---
title: Alt anahtar türetme ve ASP.NET Core kimliği doğrulanmış şifreleme
author: rick-anderson
description: ASP.NET Core veri koruma uygulama ayrıntılarını türetme alt anahtar ve şifreleme kimliği doğrulanmış öğrenin.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: 8c83da40a524896becc07c94c01d5e2b684e4386
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="subkey-derivation-and-authenticated-encryption-in-aspnet-core"></a>Alt anahtar türetme ve ASP.NET Core kimliği doğrulanmış şifreleme

<a name="data-protection-implementation-subkey-derivation"></a>

Çoğu anahtar halkası anahtarlarında entropi çeşit içerir ve algoritmik bilgiler belirten "CBC modunda şifreleme + HMAC doğrulama" veya "GCM şifreleme + doğrulama". Bu durumda, biz bu anahtar için ana anahtar malzemesini (veya KM) olarak katıştırılmış entropi başvurun ve şu gerçek şifreleme işlemleri için kullanılan anahtarları türetmek için bir anahtar türetme işlevi gerçekleştirir.

> [!NOTE]
> Anahtarları soyut ve özel bir uygulama olarak aşağıdaki davranabilir değil. Anahtar kendi uyarlamasını sağlıyorsa `IAuthenticatedEncryptor` bizim yerleşik oluşturucuları birini kullanmak yerine, bu bölümde açıklanan mekanizması artık uygular.

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>Ek kimliği doğrulanmış veriler ve alt anahtar türetme

`IAuthenticatedEncryptor` Arabirimi tüm kimliği doğrulanmış şifreleme işlemleri için temel arabirim olarak hizmet verir. Kendi `Encrypt` yöntemi iki arabelleğini alır: düz metin ve additionalAuthenticatedData (AAD). Düz metin içeriği akış çağrısı değişmeden `IDataProtector.Protect`, ancak AAD sistem tarafından oluşturulur ve üç bileşenden oluşur:

1. 32-bit Sihirli üstbilgisi 09 F0 C9 F0'de, veri koruma sisteminin bu sürümünde tanımlar.

2. 128-bit anahtar kimliği.

3. Değişken uzunlukta bir dize biçimlendirilmiş oluşturulan amacı zincirinden `IDataProtector` bu işlemi gerçekleştirme.

AAD her üç bileşenin tanımlama grubu için benzersiz olduğundan, biz KM kendisini tüm bizim şifreleme işlemlerinin kullanmak yerine yeni anahtarlar KM türetilen için bunu kullanabilirsiniz. Her çağrı için `IAuthenticatedEncryptor.Encrypt`, aşağıdaki anahtar türetme işlem gerçekleşir:

( K_E, K_H ) = SP800_108_CTR_HMACSHA512(K_M, AAD, contextHeader || keyModifier)

NIST SP800 108 KDF sayaç modunda burada arıyoruz (bkz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), sn 5.1) aşağıdaki parametrelerle:

* Anahtar türetme anahtarı (KDK) K_M =

* PRF = HMACSHA512

* Etiket additionalAuthenticatedData =

* bağlam contextHeader = || keyModifier

İçerik Üstbilgisi değişken uzunluğu ve aslında bir parmak izi, biz K_E ve K_H türetme algoritmaları sunar. Anahtar değiştiricisi her çağrı için rastgele oluşturulan bir 128-bit dizedir `Encrypt` ve olasılık KDF için diğer tüm giriş sabit olsa bile KE ve KH bu özel kimlik doğrulama şifreleme işlemi için benzersiz olduğunu aşırı ile sağlamak için kullanılır.

CBC modunda şifreleme + HMAC doğrulama işlemleri için | K_E | simetrik blok şifreleme anahtarı uzunluğu ve | K_H | HMAC yordamının Özet boyutudur. GCM şifreleme için + doğrulama işlemleri | K_H | 0 =.

## <a name="cbc-mode-encryption--hmac-validation"></a>CBC modunda şifreleme + HMAC doğrulama

Yukarıdaki mekanizması K_E oluşturulduktan sonra size rastgele başlatma vektörü oluşturur ve simetrik blok şifreleme algoritması düz metin şifrele çalıştırın. Ciphertext ve başlatma vektörünü sonra Mac üretmek için K_H anahtarı ile başlatıldı HMAC yordamı aracılığıyla çalıştırılır Bu işlem ve dönüş değeri grafik aşağıda gösterilir.

![CBC modunda işlemi ve dönüş](subkeyderivation/_static/cbcprocess.png)

*output:= keyModifier || iv || E_cbc (K_E,iv,data) || HMAC(K_H, iv || E_cbc (K_E,iv,data))*

> [!NOTE]
> `IDataProtector.Protect` Uygulaması olacak [Sihirli üstbilgi ve anahtarı kimliği başına](xref:security/data-protection/implementation/authenticated-encryption-details) çağırana dönmeden önce çıktı. Sihirli üstbilgi ve anahtarı kimliği örtük olarak olduğundan parçası [AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad), ve anahtar değiştiricisi KDF giriş olarak gönderilir, bu Mac tarafından döndürülen yük tek her bir bitini doğrulanır anlamına gelir

## <a name="galoiscounter-mode-encryption--validation"></a>Galois/sayaç modu şifreleme + doğrulama

Yukarıdaki mekanizması K_E oluşturulduktan sonra size rastgele 96 bit nonce oluşturmak ve düz metin şifrele ve 128-bit kimlik doğrulaması etiketi üretmek için simetrik blok şifreleme algoritması çalıştırın.

![GCM modu işlemi ve dön](subkeyderivation/_static/galoisprocess.png)

*Çıkış: keyModifier = || nonce || E_gcm (K_E, nonce, veri) || authTag*

> [!NOTE]
> GCM yerel AAD kavramı desteklemesine rağmen biz yine AAD yalnızca özgün KDF GCM kendi AAD parametresi için boş bir dizeyi geçirmek için kullanmama besleme. Bunun nedeni iki Katlama ' dir. İlk olarak, [çeviklik desteklemek için](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers) hiçbir zaman K_M doğrudan şifreleme anahtarı olarak kullanmak istiyoruz. Ayrıca, GCM girdilerinden çok sıkı benzersizlik gereksinimlerine uygular. GCM şifreleme yordamı iki üzerinde çağrılan şimdiye kadar veya daha fazla ayrı olma olasılığını aynı (anahtar, nonce) giriş verisi kümelerini çifti 2 aşmamalıdır ^ 32. Biz K_E düzeltin, 2'den fazla gerçekleştiremiyoruz ^ biz çalıştırmak afoul 2 ve önce 32 şifreleme işlemlerini ^ -32 sınırlayın. Bu çok sayıda işlemleri gibi görünebilir, ancak bir yüksek trafik web sunucusu da bu anahtarları normal etkin kalma süresi içinde yalnızca gün 4 milyar istekleri üzerinden gidebilirsiniz. 2 uyumlu kalmak için ^-32 olasılık sınırı, biz 128-bit anahtar değiştirici ve tüketicisinin herhangi verilen K_M kullanılabilir işlem sayısını genişletir 96 bit nonce kullanmaya devam eder. Tasarım kolaylık için biz KDF kod yolu CBC ve GCM işlemleri arasında paylaşın ve AAD KDF zaten kabul beri GCM yordamına iletmek için gerek yoktur.

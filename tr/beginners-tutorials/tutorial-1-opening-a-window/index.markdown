---
layout: tutorial
status: publish
published: true
title: 'Tutorial 1 : Opening a window'
date: '2011-04-07 17:54:16 +0200'
date_gmt: '2011-04-07 17:54:16 +0200'
categories: [tuto]
order: 10
tags: []
---

* TOC
{:toc}

# Giriş

İlk eğitime hoş geldiniz!

OpenGL'e geçmeden önce, her bir eğitimle birlikte gelen kodu nasıl oluşturacağınızı, nasıl çalıştıracağınızı ve en önemlisi kodla kendiniz nasıl oynayacağınızı öğreneceksiniz.

# Ön Koşullar

Bu eğitimleri takip edebilmek için özel bir ön koşul bulunmamaktadır. Experience with any programming langage ( C, Java, Lisp, Javascript, whatever ) is better to fully understand the code, but not needed; it will merely be more complicated to learn two things at the same time.

Bütün eğitimler "Easy C++" ile yazıldı: Kodu mümkün olduğunca basit hale getirmek için çok çaba harcanmıştır. Şablon yok, sınıf yok, pointer yok. Bu şekilde, sadece Java'yı biliyorsanız bile her şeyi anlayabileceksiniz.

# Her Şeyi Unut 

Hiçbir şey bilmek zorunda değilsin, ama OpenGL hakkında bildiğin herşeyi unutmalısın. glBegin() gibi bir şeyler biliyorsan, unut gitsin. Burada modern OpenGL (OpenGL 3 ve 4) , ve bir çok online eğitimler "eski" OpenGL (OpenGL 1 ve 2) yi öğretir. Kafanız fazla karışmadan bildiğiniz ne varsa unutun.

# Eğitimlerin oluşturulması

Bütün eğitimler Windows, Linux ve Mac üzerinde oluşturulabilinir. Bütün platformlar için, prosedür kabaca aynıdır :

* **Sürücülerini Güncelle** !! yaaap. Uyarıldın.
* Bir derleyicin yoksa derleyici indirin.
* CMake'i kurun
* Eğitimlerin kaynak kodlarını indirin.
* CMake kullanarak bir proje oluşturun 
* Projeyi derleyicinizi kullanarak Build edin
* Örneklerle oynayın!

Her platform için detaylı procedürler şimdi verilecektir. Uyarlamalar gerekli olabilir. Emin değilseniz, Windows için talimatları okuyun ve bunları uyarlamaya çalışın.

## Windows üzerinde kurulum

 

* Sürücüleri güncellemek kolay olmalı. NVIDIA' nın ya da AMD' nin web sayfasına gidin ve sürücüleri indirin. Eğer GPU modelinizden emin değilseniz : Control Panel -> System and Security -> System -> Device Manager -> Display adapter. Entegre bir Intel GPU' nuz varsa, sürücüler genellikle OEM(Dell, HP, ...)' niz tarafından sağlanır.
* Visual Studio 2017 Express' i masaüstü derleyicisi olarak kullanmanızı öneririz. Ücretsiz olarak [buradan](https://www.visualstudio.com/en-US/products/visual-studio-express-vs) indirebilirsiniz.  Özel kurulum seçmenizden ve C++' ı tercih etmiş olmanızdan emin olun. MinGW kullanmayı tercih ederseniz, [Qt Creator](http://qt-project.org/) kullanmanızı öneririz. İstediğinizi yükleyin, Sonraki adımlar Visual Studio ile açıklanacaktır, ancak diğer IDE'lerle benzer olmalıdır.
* CMake'i [buradan](http://www.cmake.org/cmake/resources/software.html) indirin ve yükleyin.
* [Kaynak kodu indirin](http://www.opengl-tutorial.org/download/) ve zipli dosyadan çıkartın, C:\Users\XYZ\Projects\OpenGLTutorials\ dizinine indirebilirsiniz. 
* CMake'i başlatın. İlk satırda, sıkıştırılmamış klasöre gidin. Emin değilsen,CMakeLists.txt dosyasını içeren klasörü seç. İkinci satırda, bütün derleyicinin işlerini yapacağı yeri girin. Örnek olarak C:\Users\XYZ\Projects\OpenGLTutorials-build-Visual2017-64bits\ ya da  C:\Users\XYZ\Projects\OpenGLTutorials\build\Visual2017-32bits\ klasör yolunu seçebilirsiniz. Her yerde olabileceğine dikkat edin, aynı klasör olması önemli değildir.![]({{site.baseurl}}/assets/images/tuto-1-window/CMake.png)

* Configure butonuna tıklayın. Projeyi ilk kez konfigürasyon yapmak istediğinizde CMake size hangi derleyiciyi kullanacağınızı soracaktır. 1. adıma göre dikkatlice seçin. Eğer 64 bit bilgisayarınız varsa, 64 bit seçebilirsiniz; bilmiyorsanız 32 bit seçebilirsiniz.
* Bütün kırmızı çizgiler kaybolana kadar Configure butonuna basınız. Generate butonuna basınız. Visual Studio projeniz şimdi oluşturuldu. Şimdi CMake'i unutabilirsin.
* C:\Users\XYZ\Projects\OpenGLTutorials-build-Visual2010-32bits\ dizinini açın. Tutorials.sln dosyasını göreceksiniz : Visual Studio ile açın.
![]({{site.baseurl}}/assets/images/tuto-1-window/directories.png)

*Build* menü içerisinde, *Build All* 'ı tıklayın. Her eğitim ve bağımlılıkları derlenecektir. Her çalıştırılabilir dosya C:\Users\XYZ\Projects\OpenGLTutorials\  dizini içerisine kopyalanacaktır. Umarım hiçbir hata olmaz.
![]({{site.baseurl}}/assets/images/tuto-1-window/visual_2010.png)

* C:\Users\XYZ\Projects\OpenGLTutorials\playground dizinini açın ve playground.exe'yi çalıştırın. Siyah bir pencere görünmeli.
![]({{site.baseurl}}/assets/images/tuto-1-window/empty_window.png)


Ayrıca herhangi bir eğitimi Visual Studio'dan da çalıştırabilirsin. Playground üzerinde sağa tıklayıp "Choose as startup project" seçerek yapabiliriz. Şimdi kodu F5 ile debug edebiliriz.

![]({{site.baseurl}}/assets/images/tuto-1-window/StartupProject.png)











## Linux üzerinde kurulum

Her bir olası platformu listelemenin mümkün olmadığı çok fazla Linux varyasyonu vardır. Gerekirse uyarlayın ve dağıtımınızın dökümanlarını okumaktan çekinmeyin.

 

* En son sürücüleri yükleyin. Install the latest drivers. Kapalı kaynak binary sürücüleri tavsiye ediyoruz. GNU değil ya da her neyse, ama çalışıyorlar. Dağıtımınız otomatik yükleme sağlamıyorsa, [Ubuntu'nun rehberini](http://help.ubuntu.com/community/BinaryDriverHowto) deneyiniz.
* Bütün gerekli derleyicileri, araçları ve kütüphaneleri yükleyin. Bütün liste : *cmake make g++ libx11-dev libxi-dev libgl1-mesa-dev libglu1-mesa-dev libxrandr-dev libxext-dev libxcursor-dev libxinerama-dev libxi-dev* . `sudo apt-get install *****` or `su && yum install ******` komutları ile kullanabilirsiniz.
* [Kaynak kodu indirin](http://www.opengl-tutorial.org/download/) ve zip dosyasından çıkartın,  örnek olarak ~/Projects/OpenGLTutorials/ dizinine çıkartabilirsiniz. 
* cd ile ~/Projects/OpenGLTutorials/ dizinine gidin ve aşağıdaki komutları girin:

 * mkdir build
 * cd build
 * cmake ..


* build/ dizininde bir makefile dosyası oluşturulmuştur.
* "make all" yazın. Her eğitim ve gereklilikler derlenmiş olacaktır. Her çalıştırılabilir  dosya ~/Projects/OpenGLTutorials/ dizinine kopyalanacaktır. Umarım hiçbir hata oluşmaz.
* ~/Projects/OpenGLTutorials/playground  dizinini açın, ./playground çalıştırın. Bir siyah pencere görünecektir. A black window should appear.

[Qt Creator](http://qt-project.org/) gibi bir IDE kullanmanız gerektiğini unutmayın. Özellikle, CMake içiin built-in destek vardır ve debug yaparken daha güzel bir deneyim sağlayacaktır. QtCreator için kullanım talimatları :

* QtCreator içerisinde, File->Tools->Options->Compile&Execute->CMake dizinine gidin
* CMake yolunu belirtin. Büyük olasılıkla /usr/bin/cmake olacaktır
* File->Open Project; tutorials/CMakeLists.txt dosyasını seçiniz
* Derleme yapılacak klasörü seçiniz , tercihen eğitim klasörünün dışında
* İsteğe bağlı olarak -DCMAKE_BUILD_TYPE=Debug parametre kutusundan ayarlayabilirsiniz. Doğrulayın.
* Alttaki çekiç üzerine tıklayın. Eğitimler artık tutorials/ klasöründen başlatılabilir.
* Eğitimleri QtCreatorden çalıştırmak içi, Projects->Execution parameters->Working Directory' ye tıklayınız ve  shaderların, textureların & modellerin bulunduğu dizini seçin. Örnek tutorial 2 için : ~/opengl-tutorial/tutorial02_red_triangle/


## Mac üzerinde kurulum

Prosedür Windows'a çok benziyor. (Makefile dosyalarıda destekleniyor, ancak burada açıklanmayacaktır.) :

* XCode uygulamasını Mac App Store'dan indirin. 
* [CMake'i indirin](http://www.cmake.org/cmake/resources/software.html), ve .dmg yi yükleyin. Komut satırı araçlarını indirmenize gerek yoktur.
* [Kaynak kodu indirin](http://www.opengl-tutorial.org/download/) ve zip içerisinden çıkartın, örnek olarak ~/Projects/OpenGLTutorials/ bu dizine çıkartabilirsiniz .
* CMake'i çalıştırın (Applications->CMake). İlk satırda, sıkıştırılmamış klasöre gidin. Emin değilsen,CMakeLists.txt dosyasını içeren klasörü seçin.  İkinci satırda, bütün derleyicinin işlerini yapacağı yeri girin. Örnek olarak ~/Projects/OpenGLTutorials_bin_XCode/ klasör yolunu seçebilirsiniz. Her yerde olabileceğine dikkat edin, aynı klasör olması önemli değildir.!
* Configure butonuna tıklayın. Projeyi ilk kez konfigürasyon yapmak istediğinizde CMake size hangi derleyiciyi kullanacağınızı soracaktır. Xcode'u seçin.
* Bütün kırmızı çizgiler kaybolana kadar Configure butonuna basınız. Generate butonuna basınız. XCode projeniz şimdi oluşturuldu, CMake'i unutabilirsiniz.
* ~/Projects/OpenGLTutorials_bin_XCode/ dizinine gidin. Tutorials.xcodeproj dosyasını göreceksiniz : dosyayı açın.
* XCode'un Schema panelinde çalıştırmak için istediğiniz eğitimi seçin, derlemek ve çalıştırmak için Run butonunu kullanın:

![]({{site.baseurl}}/assets/images/tuto-1-window/Xcode-projectselection.png)


## Code::Blocks için Not

2 hata nedeniyle(biri C::B, bir tane CMake'de), komut satırını Project->Build Options->Make komutlarında aşağıdaki gibi düzenleyiniz :

![]({{site.baseurl}}/assets/images/tuto-1-window/CodeBlocksFix.png)


Ayrıca çalışma dizininizi kendiniz ayarlamanız gerekiyor: Project->Properties -> Build targets -> tutorial N -> execution çalışma klasörü ( bu src_dir/tutorial_N/ klasördür. ).

# Eğitimleri Çalıştırma

Eğitimleri doğruca doğru dizinden çalıştırmalısınız: sadece çalıştırılabilir dosyaya çift tıklayın. Komut satırını seviyorsanız, cd komutu ile doğru dizine gidin.

Eğitimleri IDE ile çalıştırmak istiyorsanız, Çalışma klasörünü doğru ayarlamak için yukarıdaki talimatları okumayı unutmayın.

# Eğitimleri Nasıl Takip Etmeliyim? 

Her eğitim tutorialXX/ dizini içerisinde bulabileceğiniz kendi kaynak kodu ve datası ile gelir. Ancak bu projeleri asla değiştirmeyin etmeyin: Bunlar sadece referans içindir. playground/playground.cpp dosyasını açın ve bu dosya üzerinde değişiklik yapın. İstediğiniz şekilde değişiklik yapabilirsiniz. Eğer kafanız karışır ve işin içinden çıkamazsanız, sadece hangi eğitimde iseniz onun kodlarını kopyalayın ve yapıştırın, her şey normale dönecektir.

Bütün eğitimler boyunca kod parçaları sunacağız. Okurken onları playgorund alanına doğrudan kopyalayıp yapıştırmaya tereddür etmeyin, denemek iyidir. Bitmiş kodu okumaktan kaçının, bu şekilde çok şey öğrenemeyeceksiniz. Sadece kopyala yapıştırma ile bile çok rahat öğrenebilirsiniz.

# Pencere Açmakwindow

Sonunda ! OpenGL kod !
Şey, aslında değil, Birçok eğitim size, bir şeyler yapmanın "low lewel" yolunu gösterir, böylece hiç bir sihrin gerçekleşmediğini görebilirsiniz. Ama "open a window" kısmı aslında çok sıkıcı ve işe yaramaz, bu yüzden bunun yerine bunu yapmak için bir external kütüphane olan GLFW'yi kullanacağız. Gerçekten istiyorsanız, Windows üzerinde Win32 API'sini, Linux'ta X11 API'sini ve Mac'te Cocoa API'sini kullanabilirsiniz; veya SFML, FreeGLUT, SDL gibi başka bir yüksek seviye kütüphane kullanabilirsiniz. [Buradan](http://www.opengl-tutorial.org/miscellaneous/useful-tools-links/) inceleyebilirsiniz.

Hadi başlayalım. İlk olarak, bağımlılıklarla uğraşmak zorundayız: konsoldaki mesajları görüntülemek için bazı temel şeylere ihtiyacımız var:

``` cpp
// Standart kütüphaneleri ekliyoruz.
#include <stdio.h>
#include <stdlib.h>
```

İlk olarak, GLEW. Bu aslında biraz büyülü, daha sonra bunu detaylı inceleyeceğiz.

``` cpp
// GLEW kütüphanesini ekleyin. Her zaman gl.h ve glfw3.h kütüphanesinden önce ekleyin.
#include <GL/glew.h>
```

Pencereyi ve klavyeyi kullanmamız için gerekli olan GLFW kütüphanesini ekliyoruz.

``` cpp
// GLFW kütüphanesinin eklenmesi
#include <GLFW/glfw3.h>
```

Bu kütüphaneye şimdilik ihtiyacımız yok ancak bu kütüphane 3B matematik kütüphanesidir. Faydasını ilerleyen eğitimlerde göreceğiz. Herhangi bir ayrıcalığı yoktur GLM'nin eğer isterseniz kendiniz de yazabilirsiniz. Kullanışlı bir kütüphanedir. "using namespace", "glm::vec3" yazmak yerine "vec3" yazmak için eklenmiştir.

``` cpp
// GLM kütüphanesini ekliyoruz.
#include <glm/glm.hpp>
using namespace glm;
```

Playground.cpp dosyasına bütün #include kütüphanelerini kopyalayıp yapıştırdıktan sonra derleyici main() fonksiyonun olmadığını hata olarak verecektir. main() fonksiyonunu oluşturalım :

``` cpp
int main(){
```

İlk olarak GLFW' yi yüklemeliyiz :

``` cpp
// GLFW yüklenmesi
glewExperimental = true; // Core Profil için gerekli
if( !glfwInit() )
{
    fprintf( stderr, "Failed to initialize GLFW\n" );
    return -1;
}
```

Şimdi ilk OpenGL penceremizi oluşturabiliriz !

 

``` cpp
glfwWindowHint(GLFW_SAMPLES, 4); // 4x antialiasing
glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3); // OpenGL 3.3 
glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE); // MacOS için kullanılabilinir, kullanılmasa da olur.
glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE); // OpenGL' in eski versiyonunu istemiyoruz. 

// Pencere açıyoruz ve OpenGL kaynaklarını oluşturuyoruz.
GLFWwindow* window; // (In the accompanying source code, this variable is global for simplicity)
window = glfwCreateWindow( 1024, 768, "Tutorial 01", NULL, NULL);
if( window == NULL ){
    fprintf( stderr, "Failed to open GLFW window. If you have an Intel GPU, they are not 3.3 compatible. Try the 2.1 version of the tutorials.\n" );
    glfwTerminate();
    return -1;
}
glfwMakeContextCurrent(window); // Initialize GLEW
glewExperimental=true; // Needed in core profile
if (glewInit() != GLEW_OK) {
    fprintf(stderr, "Failed to initialize GLEW\n");
    return -1;
}
```

Bu kodu derleyip çalıştıralım. Bir pencerenin açılıp ardından kapandığını göreceksiniz. Kullanıcının Escape tuşuna basana kadar beklemesine ihtiyacımız var:

``` cpp
// Aşağıda Escape tuşuna basıldığını yakalamamız için gereklidir.
glfwSetInputMode(window, GLFW_STICKY_KEYS, GL_TRUE);

do{
    // Clear the screen. It's not mentioned before Tutorial 02, but it can cause flickering, so it's there nonetheless.
    glClear( GL_COLOR_BUFFER_BIT );

    // Draw nothing, see you in tutorial 2 !

    // Swap buffers
    glfwSwapBuffers(window);
    glfwPollEvents();

} // Check if the ESC key was pressed or the window was closed
while( glfwGetKey(window, GLFW_KEY_ESCAPE ) != GLFW_PRESS &&
       glfwWindowShouldClose(window) == 0 );
```

Ve bu bizim ilk eğitimimizi tamamlıyor! Eğitim 2'de bir üçgenin nasıl çizildiğini göreceksiniz.

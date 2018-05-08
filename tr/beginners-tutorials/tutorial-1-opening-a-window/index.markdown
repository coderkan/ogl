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

# Building the tutorials

Bütün eğitimler Windows, Linux ve Mac üzerinde oluşturulabilinir. Bütün platformlar için, prosedür kabaca aynıdır :

* **Sürücülerini Güncelle** !! yaaap. Uyarıldın.
* Bir derleyicin yoksa derleyici indirin.
* CMake'i kurun
* Eğitimlerin kaynak kodlarını indirin.
* CMake kullanarak bir proje oluşturun 
* Projeyi derleyicinizi kullanarak Build edin
* Örneklerle oynayın!

Her platform için detaylı procedürler şimdi verilecektir. Uyarlamalar gerekli olabilir. Emin değilseniz, Windows için talimatları okuyun ve bunları uyarlamaya çalışın.

## Building on Windows

 

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











## Building on Linux

Her bir olası platformu listelemenin mümkün olmadığı çok fazla Linux varyasyonu vardır. Gerekirse uyarlayın ve dağıtımınızın dökümanlarını okumaktan çekinmeyin.

 

* Install the latest drivers. We highly recommend the closed-source binary drivers. It's not GNU or whatever, but they work. If your distribution doesn't provide an automatic install, try [Ubuntu's guide](http://help.ubuntu.com/community/BinaryDriverHowto).
* Install all needed compilers, tools & libs. Complete list is : *cmake make g++ libx11-dev libxi-dev libgl1-mesa-dev libglu1-mesa-dev libxrandr-dev libxext-dev libxcursor-dev libxinerama-dev libxi-dev* . Use `sudo apt-get install *****` or `su && yum install ******`.
* [Download the source code](http://www.opengl-tutorial.org/download/) and unzip it, for instance in ~/Projects/OpenGLTutorials/
* cd in ~/Projects/OpenGLTutorials/ and enter the following commands :

 * mkdir build
 * cd build
 * cmake ..


* A makefile has been created in the build/ directory.
* type "make all". Every tutorial and dependency will be compiled. Each executable will also be copied back into ~/Projects/OpenGLTutorials/ . Hopefuly no error occurs.
* Open ~/Projects/OpenGLTutorials/playground, and launch ./playground. A black window should appear.

Note that you really should use an IDE like [Qt Creator](http://qt-project.org/). In particular, this one has built-in support for CMake, and it will provide a much nicer experience when debugging. Here are the instructions for QtCreator :

* In QtCreator, go to File->Tools->Options->Compile&Execute->CMake
* Set the path to CMake. This is most probably /usr/bin/cmake
* File->Open Project; Select tutorials/CMakeLists.txt
* Select a build directory, preferably outside the tutorials folder
* Optionally set -DCMAKE_BUILD_TYPE=Debug in the parameters box. Validate.
* Click on the hammer on the bottom. The tutorials can now be launched from the tutorials/ folder.
* To run the tutorials from QtCreator, click on Projects->Execution parameters->Working Directory, and select the directory where the shaders, textures & models live. Example for tutorial 2 : ~/opengl-tutorial/tutorial02_red_triangle/


## Building on Mac

The procedure is very similar to Windows' (Makefiles are also supported, but won't be explained here) :

* Install XCode from the Mac App Store
* [Download CMake](http://www.cmake.org/cmake/resources/software.html), and install the .dmg . You don't need to install the command-line tools.
* [Download the source code](http://www.opengl-tutorial.org/download/) and unzip it, for instance in ~/Projects/OpenGLTutorials/ .
* Launch CMake (Applications->CMake). In the first line, navigate to the unzipped folder. If unsure, choose the folder that contains the CMakeLists.txt file. In the second line, enter where you want all the compiler's stuff to live. For instance, you can choose ~/Projects/OpenGLTutorials_bin_XCode/. Notice that it can be anywhere, not necessarily in the same folder.
* Click on the Configure button. Since this is the first time you configure the project, CMake will ask you which compiler you would like to use. Choose Xcode.
* Click on Configure until all red lines disappear. Click on Generate. Your Xcode project is now created. You can forget about CMake.
* Open ~/Projects/OpenGLTutorials_bin_XCode/ . You will see a Tutorials.xcodeproj file : open it.
* Select the desired tutorial to run in Xcode's Scheme panel, and use the Run button to compile & run :

![]({{site.baseurl}}/assets/images/tuto-1-window/Xcode-projectselection.png)


## Note for Code::Blocks

Due to 2 bugs (one in C::B, one in CMake), you have to edit the command-line in Project->Build Options->Make commands, as follows :

![]({{site.baseurl}}/assets/images/tuto-1-window/CodeBlocksFix.png)


You also have to setup the working directory yourself : Project->Properties -> Build targets -> tutorial N -> execution working dir ( it's src_dir/tutorial_N/ ).

# Running the tutorials

You should run the tutorials directly from the right directory : simply double-click on the executable. If you like command line best, cd to the right directory.

If you want to run the tutorials from the IDE, don't forget to read the instructions above to set the correct working directory.

# How to follow these tutorials

Each tutorial comes with its source code and data, which can be found in tutorialXX/. However, you will never modify these projects : they are for reference only. Open playground/playground.cpp, and tweak this file instead. Torture it in any way you like. If you are lost, simply cut'n paste any tutorial in it, and everything should be back to normal.

We will provide snippets of code all along the tutorials. Don't hesitate to cut'n paste them directly in the playground while you're reading : experimentation is good. Avoid simply reading the finished code, you won't learn a lot this way. Even with simple cut'n pasting, you'll get your boatload of problems.

# Opening a window

Finally ! OpenGL code !
Well, not really. Many tutorials show you the "low level" way to do things, so that you can see that no magic happens. But the "open a window" part is actually very boring and useless, so we will use GLFW, an external library, to do this for us instead. If you really wanted to, you could use the Win32 API on Windows, the X11 API on Linux, and the Cocoa API on Mac; or use another high-level library like SFML, FreeGLUT, SDL, ... see the [Links](http://www.opengl-tutorial.org/miscellaneous/useful-tools-links/) page.

Ok, let's go. First, we'll have to deal with dependencies : we need some basic stuff to display messages in the console :

``` cpp
// Include standard headers
#include <stdio.h>
#include <stdlib.h>
```

First, GLEW. This one actually is a little bit magic, but let's leave this for later.

``` cpp
// Include GLEW. Always include it before gl.h and glfw3.h, since it's a bit magic.
#include <GL/glew.h>
```

We decided to let GLFW handle the window and the keyboard, so let's include it too :

``` cpp
// Include GLFW
#include <GLFW/glfw3.h>
```

We don't actually need this one right now, but this is a library for 3D mathematics. It will prove very useful soon. There is no magic in GLM, you can write your own if you want; it's just handy. The "using namespace" is there to avoid typing "glm::vec3", but "vec3" instead.

``` cpp
// Include GLM
#include <glm/glm.hpp>
using namespace glm;
```

If you cut'n paste all these #include's in playground.cpp, the compiler will complain that there is no main() function. So let's create one :

``` cpp
int main(){
```

First thing to do it to initialize GLFW :

``` cpp
// Initialise GLFW
glewExperimental = true; // Needed for core profile
if( !glfwInit() )
{
    fprintf( stderr, "Failed to initialize GLFW\n" );
    return -1;
}
```

We can now create our first OpenGL window !

 

``` cpp
glfwWindowHint(GLFW_SAMPLES, 4); // 4x antialiasing
glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3); // We want OpenGL 3.3
glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE); // To make MacOS happy; should not be needed
glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE); // We don't want the old OpenGL 

// Open a window and create its OpenGL context
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

Build this and run. A window should appear, and be closed right away. Of course! We need to wait until the user hits the Escape key :

``` cpp
// Ensure we can capture the escape key being pressed below
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

And this concludes our first tutorial ! In Tutorial 2, you will learn how to actually draw a triangle.

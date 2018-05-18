---
layout: tutorial
status: publish
published: true
title: 'Eğitim 2 : Üçgen Oluşturmak'
date: '2011-04-07 18:54:11 +0200'
date_gmt: '2011-04-07 18:54:11 +0200'
categories: [tuto]
order: 20
tags: []
---

* TOC
{:toc}

Bu uzun bir eğitim olacak.

OpenGL 3 karmaşık şeyler yazmamızı kolaylaştırır, ancak basit bir üçgen çizmek aslında oldukça zordur.

Kodu düzenli olarak yapıştırmayı unutmayın.

**<span style="color: red">Program başlangıçta çalışmıyorsa, büyük ihtimal yanlış çalışma klasöründe çalışıyorsunuzdur. Dikkatlice  [ilk eğitim]({{ site.baseurl }}/beginners-tutorials/tutorial-1-opening-a-window/)'i okuyun ve [SSS]({{ site.baseurl }}/miscellaneous/faq/) Visual Studio' nun nasıl konfigürasyon yapıldığını okuyun!</span>**

# VAO (Vertex Array Object)

Çok fazla detaya girmeyeceğim, ama bir Vertex Array Object yaratmalısınız ve ayarlamalısınız :

``` cpp
GLuint VertexArrayID;
glGenVertexArrays(1, &VertexArrayID);
glBindVertexArray(VertexArrayID);
```

Pencere oluşturulduktan sonra bunu yapın (= OpenGL Context oluşturulmasından sonra) ve diğer OpenGL methodlarının çağrılmasından önce.

Eğer gerçekten VAO'lar hakkında daha fazla bilgi sahibi olmak istiyorsanız, orada birkaç başka eğitim bulunmakta, ama bu çok önemli değil.

# Ekran Koordinatları

Bir üçgen üç nokta ile tanımlanır. 3 Boyutlu grafiklerde "noktalar"(points) üzerinde konuşurken, genellikle "vertex" ( "vertices" çoğullarında ) sözcüğünü kullanırız. Bir vertex 3 koordinata sahiptir : X, Y ve Z. Bu üç koordinatı aşağıdaki şekilde düşünebilirsiniz:

- X sağında
- Y yukarı
- Z arkaya doğru(evet, arkanda, önünde değil)

Bunu daha görselleştirebilmek için daha iyi bir yol var: Sağ El Kuralı'nı kullanın.

- X senin parmağın is your thumb
- Y senin indeksin is your index
- Z senin orta parmağın. Eğer baş parmağınızı sağa ve indeksinizi gökyüzüne tutarsanız, sizin arkanızı gösterecektir.

Z'nin bu yönde olması garip, neden böyle? Kısaca : Sağ El Kuralı 100 yıl boyunca Matematikte çok sayıda yarar sağlamıştır size de yararlı araç verecektir. Tek dezavantajı sezgisel olmayan Z.

Bir yan notta, elinizi serbestçe hareket  ettirebileceğinizi unutmayın: X, Y ve Z'niz de hareket edecektir. Daha sonra bunun üzerine konuşacağız.

Yani bir üçgen yapmak için 3B noktaya ihtiyacımız var; Hadi başlayalım :

``` cpp
// 3 noktayı temsil eden 3 vektör dizisi 
static const GLfloat g_vertex_buffer_data[] = {
   -1.0f, -1.0f, 0.0f,
   1.0f, -1.0f, 0.0f,
   0.0f,  1.0f, 0.0f,
};
```

Birinci vertex (-1,-1,0)'dir. Bunun anlamı bir şekilde dönüştürmedikçe, ekranda (-1,-1) olarak görüntülenecek anlamına gelir. Bu ne anlama geliyor?  Ekran orijin bölgesi ortada, X sağda, her zamanki gibi Y yukarı. Geniş ekranda verdiği şey budur :

![screenCoordinates]({{site.baseurl}}/assets/images/tuto-2-first-triangle/screenCoordinates.png){: height="165px" width="300px"}

Bu değiştiremeyeceğiniz bir şeydir, grafik kartınızda yerleşiktir. Dolayısıyla (-1,-1) ekranın sol alt köşesinde yer alır, (1,-1) sağ üst köşede ve (0,1) ise orta üstte yer alır. Yani bu üçgen ekranın çoğunu kaplamalı.

# Üçgenin Çizilmesi

Bir sonraki adım bu üçgeni OpenGL'e vermektir. Bunu bir buffer oluşturarak yaparız:

```cpp
// Bu bizim vertex buffer'ı mızı tanımlayacak
GLuint vertexbuffer;
// 1 buffer üretin, sonuç tanımlayıcısını vertexbuffer'a yerleştirin.
glGenBuffers(1, &vertexbuffer);
// Aşağıdaki komutlar bizim 'vertexbuffer''ımızla konuşacaktır.
glBindBuffer(GL_ARRAY_BUFFER, vertexbuffer);
// vertices'lerimizi OpenGL'e veriyoruz.
glBufferData(GL_ARRAY_BUFFER, sizeof(g_vertex_buffer_data), g_vertex_buffer_data, GL_STATIC_DRAW);
```

Bunun sadece bir kez yapılması gerekiyor.

Şimdi, ana döngümüzde, hiçbir şey çizmediğimiz yerde muhteşem üçgenimizi çizebiliriz:

``` cpp
// 1st attribute buffer : vertices
glEnableVertexAttribArray(0);
glBindBuffer(GL_ARRAY_BUFFER, vertexbuffer);
glVertexAttribPointer(
   0,                  // attribute 0. No particular reason for 0, but must match the layout in the shader.
   3,                  // size
   GL_FLOAT,           // type
   GL_FALSE,           // normalized?
   0,                  // stride
   (void*)0            // array buffer offset
);
// Üçgen Çizimi
glDrawArrays(GL_TRIANGLES, 0, 3); // Starting from vertex 0; 3 vertices total -> 1 triangle
glDisableVertexAttribArray(0);
```

Şanslıysanız, sonucu beyaz olarak görebilirsiniz. (<span style="color: red">**Olmadıysa Panik yapmayınDon't panic if you don't**</span> bazı sistemler herhangi bir şey göstermek için bir shader'a ihtiyaç duyarlar :

![triangle_no_shader]({{site.baseurl}}/assets/images/tuto-2-first-triangle/triangle_no_shader1.png){: height="232px" width="300px"}

Şimdi bu biraz sıkıcı beyaz. Kırmızıya boyayarak nasıl geliştirebileceğimizi görelim. Bu shader kullanılarak yapılır.

# Shaders

## Shader Derleme

Mümkün olan en basit konfigürasyonda, iki shader'a ihtiyacınız olacak: birincisi her bir vertex için çalıştırılacak olan Vertex Shader ve her örnek için çalıştırılacak olan Fragment Shader. And 4x antialising kullandığımızdan, her bir pikselde 4 tane örneğimiz var.

Shaderlar GLSL programlama dili ile programlanır: GL Shader Language OpenGL'in bir parçasıdır. C veya Java'dan farklı olarak çalışma anında derlenir. Bunun anlamı uygulamanızı her çalıştırdığınızda, bütün shaderlarınız tekrardan derlenecektir.

The two shaders are usually in separate files. In this example, we have SimpleFragmentShader.fragmentshader and SimpleVertexShader.vertexshader . The extension is irrelevant, it could be .txt or .glsl .

So here's the code. It's not very important to fully understand it, since you often do this only once in a program, so comments should be enough. Since this function will be used by all other tutorials, it is placed in a separate file : common/loadShader.cpp . Notice that just as buffers, shaders are not directly accessible : we just have an ID. The actual implementation is hidden inside the driver.

``` cpp
GLuint LoadShaders(const char * vertex_file_path,const char * fragment_file_path){

	// Create the shaders
	GLuint VertexShaderID = glCreateShader(GL_VERTEX_SHADER);
	GLuint FragmentShaderID = glCreateShader(GL_FRAGMENT_SHADER);

	// Read the Vertex Shader code from the file
	std::string VertexShaderCode;
	std::ifstream VertexShaderStream(vertex_file_path, std::ios::in);
	if(VertexShaderStream.is_open()){
		std::stringstream sstr;
		sstr << VertexShaderStream.rdbuf();
		VertexShaderCode = sstr.str();
		VertexShaderStream.close();
	}else{
		printf("Impossible to open %s. Are you in the right directory ? Don't forget to read the FAQ !\n", vertex_file_path);
		getchar();
		return 0;
	}

	// Read the Fragment Shader code from the file
	std::string FragmentShaderCode;
	std::ifstream FragmentShaderStream(fragment_file_path, std::ios::in);
	if(FragmentShaderStream.is_open()){
		std::stringstream sstr;
		sstr << FragmentShaderStream.rdbuf();
		FragmentShaderCode = sstr.str();
		FragmentShaderStream.close();
	}

	GLint Result = GL_FALSE;
	int InfoLogLength;

	// Compile Vertex Shader
	printf("Compiling shader : %s\n", vertex_file_path);
	char const * VertexSourcePointer = VertexShaderCode.c_str();
	glShaderSource(VertexShaderID, 1, &VertexSourcePointer , NULL);
	glCompileShader(VertexShaderID);

	// Check Vertex Shader
	glGetShaderiv(VertexShaderID, GL_COMPILE_STATUS, &Result);
	glGetShaderiv(VertexShaderID, GL_INFO_LOG_LENGTH, &InfoLogLength);
	if ( InfoLogLength > 0 ){
		std::vector<char> VertexShaderErrorMessage(InfoLogLength+1);
		glGetShaderInfoLog(VertexShaderID, InfoLogLength, NULL, &VertexShaderErrorMessage[0]);
		printf("%s\n", &VertexShaderErrorMessage[0]);
	}

	// Compile Fragment Shader
	printf("Compiling shader : %s\n", fragment_file_path);
	char const * FragmentSourcePointer = FragmentShaderCode.c_str();
	glShaderSource(FragmentShaderID, 1, &FragmentSourcePointer , NULL);
	glCompileShader(FragmentShaderID);

	// Check Fragment Shader
	glGetShaderiv(FragmentShaderID, GL_COMPILE_STATUS, &Result);
	glGetShaderiv(FragmentShaderID, GL_INFO_LOG_LENGTH, &InfoLogLength);
	if ( InfoLogLength > 0 ){
		std::vector<char> FragmentShaderErrorMessage(InfoLogLength+1);
		glGetShaderInfoLog(FragmentShaderID, InfoLogLength, NULL, &FragmentShaderErrorMessage[0]);
		printf("%s\n", &FragmentShaderErrorMessage[0]);
	}

	// Link the program
	printf("Linking program\n");
	GLuint ProgramID = glCreateProgram();
	glAttachShader(ProgramID, VertexShaderID);
	glAttachShader(ProgramID, FragmentShaderID);
	glLinkProgram(ProgramID);

	// Check the program
	glGetProgramiv(ProgramID, GL_LINK_STATUS, &Result);
	glGetProgramiv(ProgramID, GL_INFO_LOG_LENGTH, &InfoLogLength);
	if ( InfoLogLength > 0 ){
		std::vector<char> ProgramErrorMessage(InfoLogLength+1);
		glGetProgramInfoLog(ProgramID, InfoLogLength, NULL, &ProgramErrorMessage[0]);
		printf("%s\n", &ProgramErrorMessage[0]);
	}
	
	glDetachShader(ProgramID, VertexShaderID);
	glDetachShader(ProgramID, FragmentShaderID);
	
	glDeleteShader(VertexShaderID);
	glDeleteShader(FragmentShaderID);

	return ProgramID;
}
```

## Our Vertex Shader

Let's write our vertex shader first.
The first line tells the compiler that we will use OpenGL 3's syntax.

``` glsl
#version 330 core
```
{: .highlightglslvs }

The second line declares the input data :

``` glsl
layout(location = 0) in vec3 vertexPosition_modelspace;
```
{: .highlightglslvs }

Let's explain this line in detail :

- "vec3" is a vector of 3 components in GLSL. It is similar (but different) to the glm::vec3 we used to declare our triangle. The important thing is that if we use 3 components in C++, we use 3 components in GLSL too.
- "layout(location = 0)" refers to the buffer we use to feed the *vertexPosition_modelspace* attribute. Each vertex can have numerous attributes : A position, one or several colours, one or several texture coordinates, lots of other things. OpenGL doesn't know what a colour is : it just sees a vec3. So we have to tell him which buffer corresponds to which input. We do that by setting the layout to the same value as the first parameter to glVertexAttribPointer. The value "0" is not important, it could be 12 (but no more than glGetIntegerv(GL_MAX_VERTEX_ATTRIBS, &v) ), the important thing is that it's the same number on both sides.
- "vertexPosition_modelspace" could have any other name. It will contain the position of the vertex for each run of the vertex shader.
- "in" means that this is some input data. Soon we'll see the "out" keyword.

The function that is called for each vertex is called main, just as in C :

``` glsl
void main(){
```
{: .highlightglslvs }

Our main function will merely set the vertex' position to whatever was in the buffer. So if we gave (1,1), the triangle would have one of its vertices at the top right corner of the screen. We'll see in the next tutorial how to do some more interesting computations on the input position.

``` glsl
  gl_Position.xyz = vertexPosition_modelspace;
  gl_Position.w = 1.0;
}
```
{: .highlightglslvs }

gl_Position is one of the few built-in variables : you *have *to assign some value to it. Everything else is optional; we'll see what "everything else" means in Tutorial 4.

## Our Fragment Shader

For our first fragment shader, we will do something really simple : set the color of each fragment to red. (Remember, there are 4 fragment in a pixel because we use 4x AA)

``` glsl
#version 330 core
out vec3 color;
void main(){
  color = vec3(1,0,0);
}
```
{: .highlightglslfs }

So yeah, vec3(1,0,0) means red. This is because on computer screens, colour is represented by a Red, Green, and Blue triplet, in this order. So (1,0,0) means Full Red, no green and no blue.

# Putting it all together

Import our LoadShaders function as the last include:

```cpp
#include <common/shader.hpp>
```

Before the main loop, call our LoadShaders function:

```cpp
// Create and compile our GLSL program from the shaders
GLuint programID = LoadShaders( "SimpleVertexShader.vertexshader", "SimpleFragmentShader.fragmentshader" );
```

Now inside the main loop, first clear the screen. This will change the background color to dark blue because of the previous glClearColor(0.0f, 0.0f, 0.4f, 0.0f) call:

``` cpp
glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
```

and then tell OpenGL that you want to use your shader:

``` cpp
// Use our shader
glUseProgram(programID);
// Draw triangle...
```

... and presto, here's your red triangle !

![red_triangle]({{site.baseurl}}/assets/images/tuto-2-first-triangle/red_triangle.png){: height="231px" width="300px"}

In the next tutorial we'll learn transformations : How to setup your camera, move your objects, etc.

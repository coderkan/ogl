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

Bu shader dosyaları ayrı ayrı dosyalardır. Bu örnekte, The two shaders are usually in separate files. Bu örnekte, SimpleFragmentShader.fragmentshader ve SimpleVertexShader.vertexshader var. Dosya uzantısı .glsl ya da .txt olabilir.

İşte kod burada. So here's the code. Bunu tam olarak anlamak çok da önemli değil, çünkü bunu genellikle bir programda sadece bir kez yapıyorsunuz, bu yüzden yorumlar yeterli olmalı. Bu fonksiyon diğer eğitimlerde kullanılacağından, ayrı bir dosya olarak yerleştirilir: common/loadShader.cpp. Bufferlar gibi, shaderlarada doğrudan erişilemediğine dikkat edin: Sadece ID var. Gerçek uygulama sürücüde gizlidir.

``` cpp
GLuint LoadShaders(const char * vertex_file_path,const char * fragment_file_path){

	// Shaderların oluşturulması
	GLuint VertexShaderID = glCreateShader(GL_VERTEX_SHADER);
	GLuint FragmentShaderID = glCreateShader(GL_FRAGMENT_SHADER);

	// Vertex Shader'ın dosyadan okunması
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

	// Fragment Shader'ın dosyadan okunması
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

	// Vertex Shader derlenmesi
	printf("Compiling shader : %s\n", vertex_file_path);
	char const * VertexSourcePointer = VertexShaderCode.c_str();
	glShaderSource(VertexShaderID, 1, &VertexSourcePointer , NULL);
	glCompileShader(VertexShaderID);

	// Vertex Shader kontrolü
	glGetShaderiv(VertexShaderID, GL_COMPILE_STATUS, &Result);
	glGetShaderiv(VertexShaderID, GL_INFO_LOG_LENGTH, &InfoLogLength);
	if ( InfoLogLength > 0 ){
		std::vector<char> VertexShaderErrorMessage(InfoLogLength+1);
		glGetShaderInfoLog(VertexShaderID, InfoLogLength, NULL, &VertexShaderErrorMessage[0]);
		printf("%s\n", &VertexShaderErrorMessage[0]);
	}

	// Fragment Shader derlenmesi
	printf("Compiling shader : %s\n", fragment_file_path);
	char const * FragmentSourcePointer = FragmentShaderCode.c_str();
	glShaderSource(FragmentShaderID, 1, &FragmentSourcePointer , NULL);
	glCompileShader(FragmentShaderID);

	// Fragment Shader kontrolü
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

	// Program kontrolü
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

## Vertex Shader'ımız

Önce Vertex Shader'ımızı yazalım.
İlk satır derleyiciye OpenGL 3'ün systaxlarını kullanacağımızı belirtir.

``` glsl
#version 330 core
```
{: .highlightglslvs }

İkinci satır giriş verilerini tanımlar:

``` glsl
layout(location = 0) in vec3 vertexPosition_modelspace;
```
{: .highlightglslvs }

Bu satırı ayrıntılı açıklayalım :

- "vec3", GLSL'deki 3 bileşenin bir vektörüdür. Üçgeni tanımlamak için kullandığımız glm::vec3'e benzer(ama farklı). Önemli olan, C ++ 'da 3 bileşen kullanırsak, GLSL'de de 3 bileşen kullanıyoruz.
- "layout(location = 0)" *vertexPosition_modelspace* özniteliğini beslemek için kullandığımız buffera işaret eder. Her vertex çok sayıda özelliğe sahiptir: Bir pozisyon, bir veya birkaç renk, bir veya birkaç texture koordinatı, diğer birçok şey. OpenGL bir rengin ne olduğunu bilmiyor: sadece bir vec3 görüyor. Bu yüzden hangi buffer'ın hangi girişe karşılık geldiğini söylemeliyiz. Bunu glVertexAttribPointer'ın ilk parametresi ile aynı değere ayarlayarak yaparız. "0" değeri önemli değildir, 12 olabilir (ancak glGetIntegerv(GL_MAX_VERTEX_ATTRIBS, &v)'den fazla olamaz ), önemli olan her iki tarafta da aynı sayı olmasıdır.
- "vertexPosition_modelspace" başka bir ada sahip olabilir. Vertex shaderların çalışması için vertex pozisyonunu içerecektir.
- "in"'in anlamı giriş datasıdır. "out" kelimesinin anlamını birazdan açıklayacağız.

Her vertex için çağrılan fonksiyon, tıpkı C'deki gibi main olarak adlandırılır :

``` glsl
void main(){
```
{: .highlightglslvs }

Main fonksiyonumuz buffer'ın içinde bulunan değerleri vertex pozisyonuna set edecektir. Yani eğer (1,1) değerini verirsek, üçgenin ekranın sağ üst köşe noktalarını belirtmiş olacağız. Bir sonraki eğitimde, giriş pozisyonunda daha ilginç hesaplamalar yapmayı göreceğiz.

``` glsl
  gl_Position.xyz = vertexPosition_modelspace;
  gl_Position.w = 1.0;
}
```
{: .highlightglslvs }

gl_Position bir kaç built-in değişkenden biridir: Bazı değerleri bu değişkene atamalısınız. Diğer her şey isteğe bağlıdır; Eğitim 4'te "her şey"'in ne anlama geldiğini göreceğiz.

## Fragment Shader'ımız

İlk fragment shader'ımız için gerçekten basit bir şey yapacağız: her parçanın rengini kırmızıya ayarlayalım. (unutmayın, bir pikselde 4 fragment var çünkü 4x AA kullanıyoruz)

``` glsl
#version 330 core
out vec3 color;
void main(){
  color = vec3(1,0,0);
}
```
{: .highlightglslfs }

vec3(1,0,0)'ün anlamı kırmızı rengin anlamına gelir. Bunun nedeni, bilgisayar ekranlarında, rengin bu sırayla Kırmızı, Yeşil ve Mavi üçlü ile temsil edilmesidir. Dolayısıyla (1,0,0)'in anlamı sadece Kırmızı,  Yeşil yok ve Mavi yok.

# Hepsini Bir Araya Koyalım

LoadShaders fonksiyonumuzu en alta include olarak ekliyoruz:

```cpp
#include <common/shader.hpp>
```

Ana döngüden önce, LoadShaders fonksiyonumuzu çağırıyoruz:

```cpp
// Shader'lardan alınan GLSL programını oluştur ve derle.
GLuint programID = LoadShaders( "SimpleVertexShader.vertexshader", "SimpleFragmentShader.fragmentshader" );
```

Şimdi ana döngüye girebiliriz, ilk olarak ekranı temizleyelim. Bu önceki glClearColor(0.0f, 0.0f, 0.4f, 0.0f) çağrısı nedeniyle arka plan rengini koyu maviye dönüştürür :

``` cpp
glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
```

ve sonra OpenGL'e shaderı kullanmak istediğinizi söyleyin:

``` cpp
// Shaderımızı kullanın
glUseProgram(programID);
// Üçgen çizin...
```

... ve işte kırmızı üçgen!

![red_triangle]({{site.baseurl}}/assets/images/tuto-2-first-triangle/red_triangle.png){: height="231px" width="300px"}

Bir sonraki eğitimde dönüşümleri öğreneceğiz: Kameranızı nasıl kurarsınız, nesnelerinizi nasıl hareket ettirirsiniz vb.

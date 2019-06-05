# ConfigBuildGradle
## I.  Gradle
### 1. Overview
	Android Studio sử dụng Gradle, bộ công cụ xây dựng nâng cao, tự động hóa và quản lý quá trình xây dựng, đồng thời cho phép tùy chỉnh cấu hình linh hoạt.
	Gradle và Android plugin chạy độc lập với Android Studio, điều này có nghĩa bạn có thể xây dựng các ứng dụng Android bằng bất kỳ IDE nào miễn rằng đầu ra giống nhau.
Note: Vì Gradle và Android plugin chạy độc lập với Android Studio vì vậy cần cập nhật riêng cho các công cụ xây dựng.

### 2. Android plugin dành cho Gradle
	Hãy nhớ rằng Gradle không biên dịch mã Java hoặc Kotlin vào tệp APK.
	Android Plugin: cho phép Gradle có thể biên dịch mã của bạn viết ra vào tệp APK, sign APK bằng keys.
	Nếu không có Android Plugin thì không có cách nào cho Gradle biết làm thế nào với bất kỳ dòng mã của bạn bởi vì Android Studio và Gradle không có đầu mối để xây dựng dự án Android. Android Plugin là một thứ gì cực kỳ magic =))

## II. Gradle structure
### 1. Build configuration files
	Để dễ hình dung chúng ta tạo một project bằng Android Studio. Tất cả các file với từ “gradle” được sử dụng để cấu hình Gradle cho các dự án Android.


#### a. File settings.gradle

	Đây là file nằm trong thư mục gốc của project, là nơi bạn thông báo cho Gradle về tất cả các sub project/module của project. Điều này được thực hiện thông qua lệnh include.
	Nếu bạn thêm một module khác vào project, Android Studio sẽ tự động thêm module đó vào tệp tin này.



#### b. File build.gradle (top-level build file)
	Đây là file nằm trong thư mục gốc của project, định nghĩa các cấu hình xây dựng áp dụng cho tất cả các module trong project.
	Mặc định Gradle không bao gồm các tính năng của Android. Do đó, Google đã cung cấp một Android plugin cho Gradle để có thể dễ dàng cấu hình một Android project.

	1. Toàn bộ khối buildscript{} này được sử dụng định nghĩa kho lưu trữ Gradle và các phụ thuộc chung cho tất cả các module trong project. 
	2. Thêm một số thuộc tính bổ sung cho dự án Gradle, cho phép nó có thể truy cập được trong suốt dự án Gradle (có thể gọi là biến toàn cục). Ví dụ như trên ta thấy giá trị biến này đang được sử dụng để xác định phiên bản của plugin kotlin-gradle được import vào project.
	3. Khối repository cấu hình các kho lưu trữ mà Gradle sử dụng để tìm kiếm hoặc tải xuống các phụ thuộc. Lớp cấu hình sẵn hỗ trợ cho các kho lưu trữ từ xa như JCenter, mavenCentral(Maven Repository) và Ivy. Bạn cũng có thể sử dụng kho lưu trữ cục bộ hoặc xác định kho lưu trữ từ xa của riêng bạn.
	4. Khối dependencies cấu hình các phụ thuộc mà Gradle cần sử dụng để xây dựng dự án. Không nên include dependencies cho module ở đây.
Ví dụ như trên: thêm phụ thuộc Android plugin và Kotlin plugin.
	5. Khối allprojects: là nơi cấu hình các repository và dependencies được sử dụng bởi tất cả các module trong project như plugin hoặc thư viện của bên thứ 3. Tuy nhiên bạn nên cấu hình các phụ thuộc dành riêng cho module trong mỗi tệp build.gradle cấp module. Mặc địch các repositories là google và jcenter.
	6. Khối task: Ta có thể viết thêm các task của Gradle để làm nhiệm vụ gì đó. Ví dụ task clean sẽ xóa cây thư mục gốc khi được chạy. Khối này chạy khi click sync project hoặc clear project.
	
#### c. File app/build.gradle
	Trong một root project và có thể có nhiều sub-project (module). Đó là lý do tại sao chúng ta thấy có 2 tệp build.gradle: một cho root-project, một cho module đi kèm với project (khi chúng ta tạo thêm module thì sẽ tự động sinh ra 1 build.gradle cho module đó).
	Nằm trong thư mục project/module/, cho phép cấu hình từng module khác nhau.


	1. Đầu tiên, apply Android plugin, Android Kotlin plugin và Kotlin Android extensions
	2. Khối android: nơi cấu hình tất cả các build options dành riêng cho Android của mình. Những giá trị có thể đặt bên trong khối này: tài liệu tham khảo
		compileSdkVersion: Chỉ định cấp độ Android API nên sử dụng để compile app. Có nghĩa là app có thể sử dụng API này hoặc thấp hơn.
		defaultConfig: Khối này đóng gói các cài đặt và các mục mặc định cho tất cả các build variants. Có thể ghi đè một số thuộc tính trong main/AndroidManifest.xml. Có thể cấu hình productFlavors để ghi đè các giá trị này cho các versions khác nhau của app.
			- applicationId: Id của app khi publish lên Store, nên tham chiếu tên gói được xác định bởi thuộc tính gói trong file main/AndroidManifest.
			- minSdkVersion: Định nghĩa  minimum API level để có thể run app.
			- targetSdkVersion: Version SDK chỉ định để test app
			- versionCode: Xác định số phiên bản của app.
			- versionName: Xác định tên version của app.
	+ buildTypes: Nơi cấu hình nhiều build type khác nhau. Mặc định hệ thống định nghĩa 2 build type là debug và release và mặc định build type debug sẽ không hiển thị.
	3. dependencies: Là nơi khai báo các dependencies cần thiết cho module

#### d. gradle-wrapper.properties: Quản lý version của Gradle được sử dụng trong project. Nó sẽ tự tải và lưu trữ version Gradle. Lưu ý: version của Gradle khác version Android plugin.



#### e. gradle.properties: Gồm hai tệp thuộc tính nằm trong thư mục gốc, có thể sử dụng để thay đổi cài đặt của Gradle.
	+ gradle.properties: Có thể cấu hình Gradle cho toàn project, ví dụ như kích thước heap tối đa của Gradle daemon (org.gradle.jvmargs=-Xmx1536m). More The Build Environment



	+ local.properties: Cấu hình các thuộc tính môi trường cục bộ cho hệ thống xây dựng (đường dẫn đến cài đặt SDK). Nội dung tệp này được tạo tự động và dành riêng cho môi trường nhà phát triển cục bộ, vì vậy không nên sửa đổi nội dung trong tệp này.



### 2. Custom build configuration
#### a. Configure build type
	Khi tạo một module mới mặc định hệ thống sẽ tự động sinh hai build type là debug và release, mặc dù debug build type không hiển thị, Android Studio cấu hình nó với debuggable true. Điều này cho phép debug trên các thiết bị android bảo mật và cấu hình APK bằng khóa gỡ lỗi chung.
	Khi phát triển app nó sẽ compile ở chế độ debug theo mặc định.
	Vậy build type để làm gì? Nó quyết định rằng code sẽ được compile như thế nào. Ví dụ chúng ta muốn ký .apk bằng debug key chúng ta sẽ đặt debug configuration trong debug build type. Nếu chúng ta có code sẵn sàng phát hành, chúng ta sẽ đặt cấu hình vào release build type.

Ví dụ sau chỉ định một applicationIdSuffix (hậu tố của applicationId) cho debug build type và release build type

	Khi run app ở chế độ debug package name sẽ là com.example.gradlesample.debug, khi run app ở chế độ release thì package name là com.example.gradlesample
	Tham khảo thêm thuộc tính build types: http://google.github.io/android-gradle-dsl/current/com.android.build.gradle.internal.dsl.BuildType.html



#### b. Configure product flavors 
	Product flavors đại diện cho các phiên bản khác nhau của ứng dụng mà có thể phát hành cho người dùng.  Nghĩa là gì, lấy một ví dụ bạn đang phát triển ứng dụng cho khách hàng, mọi thứ đều ổn cho ứng dụng đó. Sau đó chủ sở hữu sản phẩm muốn bạn phát triển ứng dụng đó cho đối tượng là quản trị viên và ứng dụng này có tất cả các tính năng của ứng dụng khách hàng cộng thêm một vài tính năng truy cập vào trang thống kê với màu sắc và tài nguyên khác. Product Flavor sẽ giúp ta làm điều đó. Cùng 1 ứng dụng sẽ có các hành vi khác nhau.
	Product flavors hỗ trợ các thuộc tính tương tự như defaultConfig (thực ra defaultConfig nằm trong lớp ProductFlavor). Điều này có nghĩa là bạn có thể cung cấp cấu hình cơ sở cho tất cả các product flavor trong khối defaultConfig và mỗi product flavor có thể thay đổi bất kỳ giá trị mặc định nào, ví dụ như applicationId
	flavorDimensions: Nếu có nhiều productFlavors thì phải chỉ cho dimension cho nó.
Ví dụ: Tạo một flavor dimension với tên "version" và thêm 2 product flavors "admin", "customer", flavors cung cấp thuộc tính applicationIdSuffix và versionNameSuffix cho app.



#### c. Build variants
	Sự kết hợp giữa build type và product flavor và là cấu hình Gradle sử dụng để xây dựng ứng dụng. Ví dụ như bạn muốn xây dựng một một phiên bản “customers” với nội dung giới hạn hoặc một phiên bản “admins” với nội dung đầy đủ hơn.
	Sau khi tạo và cấu hình product flavor, quá trình đồng bộ hoàn thành Gradle sẽ tự động tạo build variants bằng cách kết hợp build type và product flavor.
	Và tên sẽ theo quy tắc <product-flavor><build-type>

Ví dụ: như trên Gradle tạo các build variants: adminDebug, adminRelease, customerDebug, customerRelease.
Tham khảo: Configure Build Variants 

* Filter variants
	Gradle kết hợp product flavor và build types để tạo build variant, sẽ có những build variant mà bạn không cần hoặc không có ý nghĩa trong bối cảnh dự án, bạn có thể loại bỏ các build variant này bằng cách sử dụng variantFilter trong file module-level build.gradle

##### Xử lý Conflicts trong dependencies
	Điều gì sẽ xảy ra khi project phụ thuộc vào các thư viện mà các thư viện đó lại phụ thuộc vào các thư viện khác. Kết quả sẽ hình thành một cây phụ thuộc.
Ví dụ: hai dependencies đều có chung phụ thuộc đến thư viện org.hamcrest:hamcrest-core với các version khác nhau:

	Câu hỏi đặt ra là nếu cả hai dependencies đều sử dụng các version khác nhau của cùng một thư viện, thì version nào sẽ được sử dụng? Trl: Version cao nhất sẽ được sử dụng
	Khi có xung đột về dependencies nhà phát triển cần quyết định phiên bản nào của thư viện sẽ dùng trong build. Có nhiều cách để giải quyết xung đột:

+ Cách 1: Loại bỏ module/lib xung đột từ dependencies
	Trong khi khai báo dependency, ta có thể chỉ định modules nào mà ta không muốn.
Ví dụ: chúng ta không muốn version mới nhất 1.3 và thay vào đó muốn version 1.1 của thư viện hamcrest, thì chúng ta có thể exclude module đó khi khai báo phụ thuộc của "junit"

Ngoài ra, nếu chúng ta muốn sử dụng cả version 1.3 trong build, thì chúng ta có thể exclude khỏi phụ thuộc "mockito"

Tham khảo: 
	Resolving Conflicts in android gradle dependencies
	Exclude transitive dependencies

+ Cách 2: Force resolution of the library
	Thay vì khai báo cho một cấu hình, chúng ta force (ép buộc) nó cho tất cả các cấu hình.

Note: Cách này nên sử dụng hết sức cẩn thận. Mặc dù nó giải quyết xung đột, nhưng có một số tác động nghiêm trọng, nếu các phụ thuộc(junit, mockito) được cập nhật theo version của thư viện "hamcrest", chúng ta sẽ vẫn buộc phải sử dụng version trước. Cách này là chúng ta đang buộc version phụ thuộc vào tất cả các cấu hình thay vào một cấu hình duy nhất.
3. Gradle-Task
	Cốt lõi của Gradle là projects và tasks
	Task: đại diện cho một phần công việc duy nhất cho một kiểu kiến trúc, ví dụ như là compiling classes, generating Javadoc... 
	Mỗi project được tạo từ một hoặc nhiều task


	Trong Gradle, các task được chia thành nhiều nhóm, có 4 nhóm chính:
		android: Các task liên quan đến dependencies
		build: Các task liên quan đến build các variant khác nhau ứng với các productFlavor khác nhau.
		install: Các task liên quan đến cài đặt app
		verification: Các task liên quan đến kiểm tra device, connect, kiến trúc,..
	Định nghĩa một task đơn giản như sau (build.gradle):

	Task sẽ không tự động chạy, ta có thể sử dụng command-line hoặc giao diện trong Android Studio.
	Một task có thể phụ thuộc vào một task khác. Ví dụ trên task "hello" được chạy khi task "clean" được chạy.

Tham khảo:	https://docs.gradle.org/current/dsl/org.gradle.api.Task.html
		https://docs.gradle.org/current/userguide/tutorial_using_tasks.html

## III. Code Coverage with Jacoco (Java Code Coverage)
	Test coverage reports là một công cụ quan trọng để đo lường bao nhiêu test của chúng ta thực sự thực hiện với code. Mặc dù không đảm bảo rằng phần mềm không có lỗi nhưng cung cấp cho ta thấy số phần trăm test bao phủ trong dự án. 
	Để tạo coverage report, sử dụng Jacoco (Java Code Coverage) - một công cụ được sử dụng phổ biến trong Java với mục đích này.
	Đối với Android có hai test artifacts khác nhau, thường được đại diện bởi hai thư mục test(unit) và androidTest (instrumented)
	Mặc định, Android plugin chỉ tạo coverage report từ instrumented tests. Để có thể tạo coverage cho unit test, ta phải tạo task thủ công với Jacoco test.

### Setting
	Trong root build.gradle: Thêm jacoco version vào classpath dependencies


	Enable coverage cho instrumented tests

	Enable coverage for UnitTests

	Trong app/build.gradle
Add plugin jacoco và thiết lập version của nó tương ứng với version được thiết lập ở build.gradle

Tạo task jacocoTestReport:
	

	Để chạy task có thể sử dụng dòng lệnh hoặc sử dụng giao diện trong Android Studio tìm đến task đó.
	Khi run các task sẽ sinh ra file dạng .html để lưu trữ dữ liệu test.


## IV. Tài liệu tham khảo
	Android developer: https://developer.android.com/studio/build
	Gradle: https://docs.gradle.org/current/userguide/userguide.html
	Medium: https://medium.com/@iammert/android-product-flavors-1ef276b2bbc1
		https://medium.com/mindorks/avoiding-conflicts-in-android-gradle-dependencies-28e4200ca235



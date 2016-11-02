### Android Gradle相关知识点

- Android Studio创建工程与Gradle有关的文件：
	- .gradle 临时目录
	- Moudle-Level build.gradle 单独Module的配置文件
		模块级script用于描述该模块的编译过程，一般而言，Android能用到的有三种：
			- Application:对应com.android.application 编译的结果是一个apk
			- Android Library:对应com.android.library 编译的结果为aar
			- Java Library: 对应的java 编译结果为jar
		#### 以application为例
		
		```
		//声明Module为主Module
		apply plug: 'com.android.application'
		
		android {
			//设置编译sdk和编译工具的版本
			complideSdkVersion 23
			buildToolsVersion '23.0.3'
			
			signingConfigs {
				//AS会自动帮我们使用 debug certificate进行签名，因此不适合作为发布使用
				debug{
				}
				//发布版本的签名文件及密钥
				release {
					
				}
			}
			
			//与清单文件中一样
			defaultConfig {
				applicationId
				minSdkVersion
				targetSdkVersion
				versionCode
				versionName
			}
			
			//打什么样的包
			buildTypes {
				//一般保持默认
				debug{
				}
				//开启混淆 使用签名
				release{
				}
			}
			/**
			 *	强调不同的版本，比如付费版和免费版
			 *	在国内，这个字段更多被用于区分不同的渠道。
			 */
			productFlavors {
				_360{
				}
				xiaomi{
				}
				...
			}
			
			//这个Module的依赖
			dependencies {
				compilde...
				complide...
				complide...
			}
		}
		
		```
		
	- Top-Level build.gradle 工程的配置文件
		 
		```
		buildscript{
			//buildscript就是运行构建脚本所需要用到的依赖的classpath.
			repositories{
				jcenter()
			}
			//构建脚本运行所依赖的文件
			dependencies{
				//依赖文件的查找地址
				classpath 'com.android.tools.build:gradle:2.2.1'
			}
		}
		``` 
		```
		//project下的所有module都会在指定仓库中寻找依赖.
		allprojects {
    		repositories {
        		jcenter()
    		}

		```
	- Gradle Wrapper 用于统一编译环境
	
		```
		//本地Gradle的安装目录 和要使用的Gradle版本，本地没有通过URL去下载。
		distributionBase=GRADLE_USER_HOME
		distributionPath=wrapper/dists
		zipStoreBase=GRADLE_USER_HOME
		zipStorePath=wrapper/dists
		distributionUrl=https\://services.gradle.org/distributions/gradle-2.14.1-all.zip
	```


  
---
sort: 3  
---  

# 3. CMake

CMakeLists.txt 이해하고, CMake로 Extension 프로젝트 빌드하기  

### 빌드 과정
인간이 이해하는 명령어의 조합인 소스파일(*.cpp)을 컴퓨터가 이해하는 기계어의 목적파일(*.o)로 변환한다. __컴파일__ 
일반적으로 하나의 프로그램은 여러 개의 목적파일들과 라이브러리들의 조합으로 이루어지는데, 이 파일들을 연결시키는 작업을 통해, __링크__ 
실행가능한 파일(*.exe, *.out 등)을 생성한다. __빌드__ 

__GCC 빌드 예(리눅스)__  
```
#main 프로젝트 생성  
gcc -c main.c  #목적파일 main.o 생성
gcc -c help.c  #목적파일 help.o 생성 
gcc -o main main.o help.o # 목적파일간 관계설정 후 실행파일 main.out 생성
```  
혹은
```
gcc -o main main.c help.c # 실행파일 main.out 생성
```  

```mermaid
graph TD;
    main-->main.o;
    main-->help.o;
    main.o-->main.c;
    main.o-->help.c;
    help.o-->help.c;
    help.o-->help.c;
```  

```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```

### Make란  
오래걸리고 반복되는 컴파일 작업을 간소화하기 위한 빌드 도구 
빌드때마다 도구로 파일간 관계를 일일히 설정해줄 필요가 없으며  
수정된 파일만 컴파일 할 수 있어서 컴파일 과정이 간단해지며, 대규모 프로젝트에 필수적이다.  

Makefile에 실행 파일을 만들기 위해 필요한 파일들의 관계를 서술해놓고  
Make 명령어로 간단하게 실행 파일을 만들어 준다.  

__Makefile 규칙__  
Target(대상파일) : Dependency(의존파일)
                 Command(명령어)  
```  
#main 프로젝트 생성  
main: main.o copy.o 
      gcc -o main main.o help.o 
main.o: main.c help.h
      gcc -c main.c 
help.o: help.c help.h
      gcc -c help.c 
```  



### CMake란  
CMake란 다양한 빌드 도구 중 하나이다.



__CMakeLists.txt 규칙__

1. CMake로 빌드하기위한 source file의 경로 최상위 폴더 안에 CMakeList.txt가 있어야한다.  
2. 이 CmakeList.txt에서 프로젝트명(\`project(프로젝트명)\`)과 하위 폴더(\`add_subdiretory(모듈명)\`)목록들을 설정해야한다.
3. 하위 폴더에도 .cpp와 같은 source file과 CMakeLists.txt가 있어야한다.  
4. 이 CMakeLists.txt에는 source file목록들을 설정해주어야한다.  

Extension Wizard로 템플릿을 생성하면 최상위 폴더와 각 모듈들에 CMakeLists.txt가 생성된다
### 최상위 폴더의 CMakeLists.txt 
```
cmake_minimum_required(VERSION 3.13.4) # 최소 CMake 버전, 사용중인 버전을 써주면된다.


project(Extension) # 프로젝트에 이름 설정

#-----------------------------------------------------------------------------
# Extension meta-information
set(EXTENSION_HOMEPAGE "https://www.slicer.org/wiki/Documentation/Nightly/Extensions/Extension")
set(EXTENSION_CATEGORY "Examples")
set(EXTENSION_CONTRIBUTORS "John Doe (AnyWare Corp.)")
set(EXTENSION_DESCRIPTION "This is an example of a simple extension")
set(EXTENSION_ICONURL "http://www.example.com/Slicer/Extensions/Extension.png")
set(EXTENSION_SCREENSHOTURLS "http://www.example.com/Slicer/Extensions/Extension/Screenshots/1.png")
set(EXTENSION_DEPENDS "NA") # Specified as a list or "NA" if no dependencies

#-----------------------------------------------------------------------------
# Extension dependencies
find_package(Slicer REQUIRED)
include(${Slicer_USE_FILE})

#-----------------------------------------------------------------------------
# Extension modules
add_subdirectory(Module)
## NEXT_MODULE

#-----------------------------------------------------------------------------
include(${Slicer_EXTENSION_GENERATE_CONFIG})
include(${Slicer_EXTENSION_CPACK})
```  

### 하위 폴더(Module)의 CMakeLists.txt  
```

#-----------------------------------------------------------------------------
set(MODULE_NAME Module)
set(MODULE_TITLE ${MODULE_NAME})

string(TOUPPER ${MODULE_NAME} MODULE_NAME_UPPER)

#-----------------------------------------------------------------------------
add_subdirectory(Logic)
add_subdirectory(Widgets)

#-----------------------------------------------------------------------------
set(MODULE_EXPORT_DIRECTIVE "Q_SLICER_QTMODULES_${MODULE_NAME_UPPER}_EXPORT")

# Current_{source,binary} and Slicer_{Libs,Base} already included
set(MODULE_INCLUDE_DIRECTORIES
  ${CMAKE_CURRENT_SOURCE_DIR}/Logic
  ${CMAKE_CURRENT_BINARY_DIR}/Logic
  ${CMAKE_CURRENT_SOURCE_DIR}/Widgets
  ${CMAKE_CURRENT_BINARY_DIR}/Widgets
  )

set(MODULE_SRCS
  qSlicer${MODULE_NAME}Module.cxx
  qSlicer${MODULE_NAME}Module.h
  qSlicer${MODULE_NAME}ModuleWidget.cxx
  qSlicer${MODULE_NAME}ModuleWidget.h
  )

set(MODULE_MOC_SRCS
  qSlicer${MODULE_NAME}Module.h
  qSlicer${MODULE_NAME}ModuleWidget.h
  )

set(MODULE_UI_SRCS
  Resources/UI/qSlicer${MODULE_NAME}ModuleWidget.ui
  )

set(MODULE_TARGET_LIBRARIES
  vtkSlicer${MODULE_NAME}ModuleLogic
  qSlicer${MODULE_NAME}ModuleWidgets
  )

set(MODULE_RESOURCES
  Resources/qSlicer${MODULE_NAME}Module.qrc
  )

#-----------------------------------------------------------------------------
slicerMacroBuildLoadableModule(
  NAME ${MODULE_NAME}
  TITLE ${MODULE_TITLE}
  EXPORT_DIRECTIVE ${MODULE_EXPORT_DIRECTIVE}
  INCLUDE_DIRECTORIES ${MODULE_INCLUDE_DIRECTORIES}
  SRCS ${MODULE_SRCS}
  MOC_SRCS ${MODULE_MOC_SRCS}
  UI_SRCS ${MODULE_UI_SRCS}
  TARGET_LIBRARIES ${MODULE_TARGET_LIBRARIES}
  RESOURCES ${MODULE_RESOURCES}
  WITH_GENERIC_TESTS
  )

#-----------------------------------------------------------------------------
if(BUILD_TESTING)
  add_subdirectory(Testing)
endif()
``` 


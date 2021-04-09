---
sort: 1
---

# 1. 3D Slicer Extension  

3D Slicer에 Extension Module 개발하기  
(C++ 윈도우 개발환경 기준)    

### 3d Slicer 개발환경 구축  

 1. [CMake (>=3.15.1)](https://cmake.org/download/)  
 2. [Git (>=1.7.10)](https://git-scm.com/download/win)  
 3. [NSIS](https://nsis.sourceforge.io/Download)  
 4. Qt5
    - [Qt installer](https://www.qt.io/download-qt-installer) 다운하고 실행  
    - Select Components에서 Qt 5.15.0에서 __MSVC 2019 64-bit, Sources, Qt WebEngine, Qt Script__ 선택  
    - License Agreement 후 Install  
	
![Qt5_15](/assets/images/tab_develope/3DSlicer/file1/Qt5_15.PNG){: width="80%"}{: center}
	
 5. Visual Studio 2019
    - [VS 설치 관리자](https://visualstudio.microsoft.com/ko/downloads/) 다운하고 실행
    - 사용가능 탭에서 에서 Visual Studio Community 2019 설치 클릭 (Enterprise나 Professional도 가능)  
    - 워크로드 탭에서 __C++을 사용한 데스크톱 개발 (MSVC v142 - VS 2019 C++ x64...이 체크되었는지 꼭 확인)__ 체크하고 설치  

![vs2019](/assets/images/tab_develope/3DSlicer/file1/vs2019.PNG){: width="80%"}{: center}  
    
참고 https://slicer.readthedocs.io/en/latest/developer_guide/build_instructions/windows.html#install-prerequisites      
 
### 3d Slicer API 받기  
Contribution 하지않는다면 생략가능    
  
1. [3dSlicer](http://slicer.kitware.com) 회원가입  
2. [NA-MIC community](https://slicer.kitware.com/midas3/community/23) 가서 _Join Community_ 클릭
3. My Account가서 API 탭 클릭
4. Default가 Application Name인 API Key 긁어서 환경변수에 저장  
   _API Key에 /가 들어가있으면 build가 안되는 issue가 발생하기때문에 해당 Key를 delete하고 regenerate하여 발급받는다_
   
   MIDAS_PACKAGE_EMAIL 에는 3d Slicer 계정 이메딜
   MIDAS_PACKAGE_API_KEY 에는 발급받은 API Key  
   
  _환경변수 반영은 cmd창을 켜서 set 명령어를 치거나 재부팅을 하면 됨_

참고 https://www.slicer.org/wiki/Documentation/Nightly/Developers/FAQ/Extensions#How_to_obtain_an_API_key_to_submit_on_the_extension_server_.3F  

### 3d Slicer 빌드 하기  

1. 3d Slicer 소스 받아오기  
  다운받고자 하는 폴더에서 오른쪽 클릭하여 _Git Bash here_ 선택 후
  \'git clone https://github.com/Slicer/Slicer.git \' 입력  
2. CMD 창에서 cmake 빌드  
   명령어에서 설정되는 경로는 환경에 맞게 변경.  
   
##### Release
```
mkdir D:\Slicer_r  #build 디렉토리 생성
cd D:\Slicer_r       #build 디렉토리 이동
cmake -G "Visual Studio 16 2019" -DQt5_DIR:PATH=D:\LIBRARY\Qt5\5.15.0\msvc2019_64\lib\cmake\Qt5 D:\Slicer
cmake --build . --config Release
``` 
##### Debug (오래걸림)
```
mkdir D:\Slicer_d  #build 디렉토리 생성
cd D:\Slicer_d       #build 디렉토리 이동
cmake -G "Visual Studio 16 2019" -DQt5_DIR:PATH=D:\LIBRARY\Qt5\5.15.0\msvc2019_64\lib\cmake\Qt5 D:\Slicer
cmake --build . --config Debug  
``` 

##### 3d Slicer 실행  

1. build 디렉토리의 가장 상위에서 Slicer.sln 찾아 visual studio 실행
2. active configuration을 빌드한 설정에 맞게 변경 Debug or Release
![active_build](/assets/images/tab_develope/3DSlicer/file1/active_build.PNG){: width="60%"}{: center}  
3. ALL_BUID 프로젝트 빌드
4. build 디렉토리/Slicer-build/Slicer.exe 클릭  

### 3d Slicer Extension Module 만들기  
1. 3D Slicer 실행하여 Extension Wizard Module 선택 
2. Create Extension 선택

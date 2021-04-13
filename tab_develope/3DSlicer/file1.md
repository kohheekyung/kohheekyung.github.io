---
sort: 1
---  
# 1. 3D Slicer 개발환경 구축

3D Slicer에 Extension Module 개발하기  
C++ 윈도우 개발환경 기준   

---
### 개발환경 

 1. [CMake (>=3.15.1)](https://cmake.org/download/)  
 2. [Git (>=1.7.10)](https://git-scm.com/download/win)  
 3. [NSIS](https://nsis.sourceforge.io/Download)  
 4. Qt5
    - [Qt installer](https://www.qt.io/download-qt-installer) 다운하고 실행  
    - Select Components에서 Qt 5.15.0에서 __MSVC 2019 64-bit, Sources, Qt WebEngine, Qt Script__ 선택  
    - License Agreement 후 Install  
	
![Qt5_15](/assets/images/tab_develope/3DSlicer/file1/Qt5_15.PNG){: width="80%"}{: .center}
	
 5. Visual Studio 2019
    - [VS 설치 관리자](https://visualstudio.microsoft.com/ko/downloads/) 다운하고 실행
    - 사용가능 탭에서 에서 Visual Studio Community 2019 설치 클릭 (Enterprise나 Professional도 가능)  
    - 워크로드 탭에서 __C++을 사용한 데스크톱 개발 (MSVC v142 - VS 2019 C++ x64...이 체크되었는지 꼭 확인)__ 체크하고 설치  

![vs2019](/assets/images/tab_develope/3DSlicer/file1/vs2019.PNG){: width="70%"}{: .center}  
    
참고 https://slicer.readthedocs.io/en/latest/developer_guide/build_instructions/windows.html#install-prerequisites      

---
### API 받기  
Contribution 하지않는다면 생략가능    
  
1. [3dSlicer](http://slicer.kitware.com) 회원가입  
2. [NA-MIC community](https://slicer.kitware.com/midas3/community/23) 가서 _Join Community_ 클릭
3. My Account가서 API 탭 클릭
4. Default가 Application Name인 API Key 긁어서 환경변수에 저장  
   _API Key에 /가 들어가있으면 build가 안되는 issue가 발생하기때문에 해당 Key를 delete하고 regenerate하여 발급받는다_
   
   MIDAS_PACKAGE_EMAIL 에는 3d Slicer 계정 이메일  
   MIDAS_PACKAGE_API_KEY 에는 발급받은 API Key  
![vs2019](/assets/images/tab_develope/3DSlicer/file1/sysPATH.PNG){: width="40%"}{: .center} 
  _환경변수 반영은 cmd창을 켜서 set 명령어를 치거나 재부팅을 하면 됨_

참조 https://www.slicer.org/wiki/Documentation/Nightly/Developers/FAQ/Extensions#How_to_obtain_an_API_key_to_submit_on_the_extension_server_.3F  

---
### Build  

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
##### Debug 
```
mkdir D:\Slicer_d  #build 디렉토리 생성
cd D:\Slicer_d       #build 디렉토리 이동
cmake -G "Visual Studio 16 2019" -DQt5_DIR:PATH=D:\LIBRARY\Qt5\5.15.0\msvc2019_64\lib\cmake\Qt5 D:\Slicer
cmake --build . --config Debug  
```   

##### Build 오류 잡기
<details>
<summary>'현재 코드 페이지(949)에서 표시할 수 없는 문자가 파일에 들어 있습니다'라는 Warning</summary>
<div markdown="1">

- LibArchive 빌드시 '현재 코드 페이지(949)에서 표시할 수 없는 문자가 파일에 들어 있습니다'라는 Warning와 함께 빌드 실패  
```
1>Performing build step for 'LibArchive'
1>.NET Framework용 Microsoft (R) Build Engine 버전 16.9.0+5e4b48a27
1>Copyright (C) Microsoft Corporation. All rights reserved.
1>
1>  archive_read_support_format_rar5.c
1>D:\slicer_project\R\LibArchive\libarchive\archive_read_support_format_rar5.c(1,1): error C2220: 다음 경고는 오류로 처리됩니다. [D:\slicer_project\R\LibArchive-build\libarchive\archive.vcxproj]
1>D:\slicer_project\R\LibArchive\libarchive\archive_read_support_format_rar5.c(1,1): warning C4819: 현재 코드 페이지(949)에서 표시할 수 없는 문자가 파일에 들어 있습니다. 데이터가 손실되지 않게 하려면 해당 파일을 유니코드 형식으로 저장하십시오. [D:\slicer_project\R\LibArchive-build\libarchive\archive.vcxproj]
1>  archive_read_support_format_rar5.c
1>D:\slicer_project\R\LibArchive\libarchive\archive_read_support_format_rar5.c(1,1): error C2220: 다음 경고는 오류로 처리됩니다. [D:\slicer_project\R\LibArchive-build\libarchive\archive_static.vcxproj]
1>D:\slicer_project\R\LibArchive\libarchive\archive_read_support_format_rar5.c(1,1): warning C4819: 현재 코드 페이지(949)에서 표시할 수 없는 문자가 파일에 들어 있습니다. 데이터가 손실되지 않게 하려면 해당 파일을 유니코드 형식으로 저장하십시오. [D:\slicer_project\R\LibArchive-build\libarchive\archive_static.vcxproj]
1>C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Microsoft\VC\v160\Microsoft.CppCommon.targets(240,5): error MSB8066: 'D:\slicer_project\R\CMakeFiles\d3e05790d5571be296b79644510db7e9\LibArchive-update.rule;D:\slicer_project\R\CMakeFiles\d3e05790d5571be296b79644510db7e9\LibArchive-patch.rule;D:\slicer_project\R\CMakeFiles\d3e05790d5571be296b79644510db7e9\LibArchive-configure.rule;D:\slicer_project\R\CMakeFiles\d3e05790d5571be296b79644510db7e9\LibArchive-build.rule;D:\slicer_project\R\CMakeFiles\d3e05790d5571be296b79644510db7e9\LibArchive-install.rule;D:\slicer_project\R\CMakeFiles\0f2b6fa69dbe7f10a9e4fca2cb61ccc8\LibArchive-complete.rule;D:\slicer_project\R\CMakeFiles\0eb6138ddcf6b35e00202fe39c2a0f3a\LibArchive.rule'에 대한 사용자 지정 빌드가 종료되었습니다(코드 1).
1>"LibArchive.vcxproj" 프로젝트를 빌드했습니다. - 실패
```   
__해결법__
build 디렉토리\LibArchive\libarchive\archive_read_support_format_rar5.c 에서 주석의 이모티콘 때문에 생긴 에러로, 한글판 Visual Studio를 사용하면 종종 주석에서 한글이나 이모티콘을 인코딩하지 못한다. 사실 인코딩 방식이 환경에 따라 달라질 수 있기 때문에 모든 소스파일엔 주석일지라도 영어만 쓰는게 좋다.  

단순하게 74번째 코드를 삭제하는 방법으로 해결할 수 있다.     

동일한 에러가 계속난다면, 해당파일을   
Visual Studio로 열고, 해당 파일을 다른 이름으로 파일 저장하기 > 저장 드롭다운 > 고급저장 옵션 > 인코딩을 유니코드 -코드 페이지 1200로 변경하여 저장하면된다.   








</div>
</details>







##### 실행  
1. build 디렉토리의 가장 상위에서 Slicer.sln 찾아 visual studio 실행
2. active configuration을 빌드한 설정에 맞게 변경 Debug or Release
![active_build](/assets/images/tab_develope/3DSlicer/file1/active_build.PNG){: width="60%"}{: center}  
3. ALL_BUID 프로젝트 빌드
4. build 디렉토리/Slicer-build/Slicer.exe 클릭  

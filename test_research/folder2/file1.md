# 3D Slicer Extension  

3D Slicer에 Extension Module 개발하기  
(C++ 윈도우 개발환경 기준)    

### 3d Slicer 개발환경 구축  

 1. CMake(>=3.15.1) https://cmake.org/download/  
 2. Git(>=1.7.10) https://git-scm.com/download/win  
 3. NSIS https://nsis.sourceforge.io/Download  
 4. Qt5
    - 인스톨러 다운하고 실행 https://www.qt.io/download-qt-installer  
    - Select Components에서 Qt 5.15.0에서 __MSVC 2019 64-bit, Sources, Qt WebEngine, Qt Script__ 선택  
    - License Agreement 후 Install  
 5. Visual Studio 2019
    - 설치 관리자 다운하고 실행 https://visualstudio.microsoft.com/ko/downloads/  
    - 사용가능 탭에서 에서 Visual Studio Community 2019 설치 클릭 (Enterprise나 Professional도 가능)  
    - 워크로드 탭에서 __C++을 사용한 데스크톱 개발 (MSVC v142 - VS 2019 C++ x64...이 체크되었는지 꼭 확인)__ 체크하고 설치  
    
참고 https://slicer.readthedocs.io/en/latest/developer_guide/build_instructions/windows.html#install-prerequisites      
 
### 3d Slicer API 받기  

1. http://slicer.kitware.com 회원가입  
2. https://slicer.kitware.com/midas3/community/23 가서 
3. My Account가서 API 탭 클릭
4. Default가 Application Name인 API Key 긁어서 환경변수에 저장  
   _API Key에 /가 들어가있으면 build가 안되는 issue가 발생하기때문에 해당 Key를 delete하고 regenerte하여 발급받는다_
   
   MIDAS_PACKAGE_EMAIL 에는 3d Slicer 계정 이메딜
   MIDAS_PACKAGE_API_KEY 에는 발급받은 API Key
   
  _환경변수 반영은 cmd창을 켜서 set 명령어를 치거나 재부팅을 하면 됨_

참고 https://www.slicer.org/wiki/Documentation/Nightly/Developers/FAQ/Extensions#How_to_obtain_an_API_key_to_submit_on_the_extension_server_.3F  

### 3d Slicer 

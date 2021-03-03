---
layout: post
title:  "Pandas Offline 설치"
search: true
categories: DataScience
last_modified_at: 2021-01-06T08:06:00-05:00
---

**1. Pandas 를 인터넷이 연결되지 않은 상태에서 사용하고 싶을 경우**<br>
1.1. whl(Wheel)은 이용한 설치
 * whl 은 Python Library Manual 설치를 지원하는 패키지다.
	* 성별, 주민번호 등
 * 설치 방법
```bash
c:\Python36-32>Python -m pip install <Wheel Package Name>
```  
 
1.2. Pandas 를 설치하기 위한 Whl 목록
 * Pandas
	* Numpy
	* python-dateutil
		*six
 
1.3. 모듈별 다운로드 경로 (Windows 기준)
 * Pandas
	* [Download URL](https://files.pythonhosted.org/packages/92/2a/a7825928dd0ddd81bd4b2dcd3723a7082c2be42645095d03cd0fe345936c/pandas-1.1.5-cp36-cp36m-win32.whl)
	* [Page Url(다른 OS 패키지 파일을 다운로드 하려면 여기로 가세요)](https://pypi.org/project/pandas/1.1.5/#files){: target="_blank"}
 * Numpy
	* [Download Url](https://files.pythonhosted.org/packages/b2/4f/5b2cdd441e92555377003b06c0c2c4ba0282a2bfe809f89a009e853fb26b/numpy-1.19.5-cp36-cp36m-win32.whl)
	* [Page Url(다른 OS 패키지 파일을 다운로드 하려면 여기로 가세요)](https://pypi.org/project/numpy/#files){: target="_blank"}
 * python-dateutil
	* [Download Url](https://files.pythonhosted.org/packages/d4/70/d60450c3dd48ef87586924207ae8907090de0b306af2bce5d134d78615cb/python_dateutil-2.8.1-py2.py3-none-any.whl)
	* OS 상관 없이 설치 가능
	* [Page Url](https://pypi.org/project/python-dateutil/#files){: target="_blank"}
 * six
	* [Download Url](https://files.pythonhosted.org/packages/ee/ff/48bde5c0f013094d729fe4b0316ba2a24774b3ff1c52d924a8a4cb04078a/six-1.15.0-py2.py3-none-any.whl)
	* OS 상관 없이 설치 가능
	* [Page Url](https://pypi.org/project/six/){: target="_blank"}
	
1.4. 설치 순서
 * 각 라이브러리들이 디펜던시가 있기 때문에 six -> python-dateutil -> numpy -> pandas 순서로 설치하면 된다.
 
```bash
c:\Python36-32>Python -m pip install six-1.15.0-py2.py3-none-any.whl

c:\Python36-32>Python -m pip install python_dateutil-2.8.1-py2.py3-none-any.whl

c:\Python36-32>Python -m pip install numpy-1.19.5-cp36-cp36m-win32.whl

c:\Python36-32>Python -m pip install pandas-1.1.5-cp36-cp36m-win32.whl
```


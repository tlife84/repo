나중에 정리

* import pandas_datareader.data 할 때 C:\ProgramData\Anaconda3\lib\site-packages\pandas_datareader\fred.py에서 cannot import name 'is_list_like' 오류 발생
pandas 버전이 업데이트 되면서 is_list_like가 pandas.api.types로 옮겨졌기 때문. fred.py에서 from pandas.core.common를 from pandas.api.types로 변경

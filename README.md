# 수입 수산물 품질인증 및 정보제공

학과 | 학번 | 성명
---- | ---- | ---- 
조선해양공학과 |201529154 |안용덕


## 프로젝트 개요
1. 우리나라로 수입되는 수산물 품목의 종류를 제공(출력) - 여기서 사용자가 상세한 품목을 볼지 간단히 나타낸 품목을 볼지 선택할 수 있음.
2. 사용자가 다양한 품목 중 하나를 입력.
3. 입력된 품목의 원산지와 품질인증 여부를 출력.
4. 사용자가 원하면 수산물 위판장의 목록을 나열.

## 사용한 공공데이터 
1.[데이터보기1](https://github.com/ayd0427/Final-HW/blob/master/inspection.csv)
2.[데이터보기2](https://github.com/ayd0427/Final-HW/blob/master/origin.csv)
3.[데이터보기3](https://github.com/ayd0427/Final-HW/blob/master/sale.zip)

## 소스
* [링크로 소스 내용 보기](https://github.com/ayd0427/Final-HW/blob/master/201529154_%EC%95%88%EC%9A%A9%EB%8D%95_%EA%B8%B0%EB%A7%90%EA%B3%BC%EC%A0%9C.py) 

* 코드 삽입
~~~python
import pandas as pd
import sys #프로그램 종료를 위해 import

df = pd.read_csv('origin.csv') #수입 원산지 제공 공공데이터

#1. 수입되는 수산물 품목의 종류를 제공(출력)
df1 = df.iloc[1773:,7] #origin.csv에서 종류 추출, 1773이 수입 처음
#print(df1) #품목들 출력

matrix = [] # 상세한 품목 종류를 저장할 리스트
for indata in df1:
    indata = indata.replace("'", "") # ' 을 공백으로 변환
    matrix.append(indata) #변환된 목록들을 리스트로 추가

matrix2 = [] # 간단한 품목 종류를 저장할 리스트
for indata1 in df1:
    indata1 = indata1.replace("'", "") # ' 을 공백으로 변환
    indata1 = indata1.split("(") # ( 를 기준으로 리스트형식으로 분리 후
    del(indata1[1:]) # 뒤에 내용 삭제
    indata1 = indata1.pop() #리스트 안에 리스트를 넣게되어서 그전에 값만 추출
    #print(indata1)
    matrix2.append(indata1)

user_num = input("품목종류(번호입력) : 1. 상세히  2. 간단히")

if user_num == '1' :
    print(matrix) #리스트화 된 상세한 목록들 출력
    print()
elif user_num == '2':
    print(matrix2) #리스트화 된 간단한  목록들 출력
    print()
else :
    print("Error")
    sys.exit()

#1-1. 사용자가 선택한 수산물 품목의 원산지 제공(출력)을 위한 리스트 정리
df2 = df.iloc[1773:,5] #origin.csv에서 원산지 추출

matrix3 = [] # 품목별 원산지를 저장할 리스트
for indata2 in df2:
    indata2 = indata2.replace("'","")
    #print(indata2)
    matrix3.append(indata2)
#print(matrix3)

#2. 사용자가 다양한 품목 중 하나를 입력.
user = input("품목입력 : ")

#3. 입력받은 품목이 matrix2 리스트 안에 있으면 원산지 및 품질인증여부 출력
if user in matrix2:
    print(user + "의 원산지는 아래와 같습니다.")
else:
    print()

k = 0 # 리스트 matrix2 에서 품목별 위치를 저장하기 위해 만든 변수
for i in matrix2:
    if i == user:
        #print(k)
        if user in matrix2:
            print(matrix3[k]) # matrix3에서 k번째의 품목의 원산지 출력
    k = k + 1

    if user not in matrix2:
        print("해당 품목 없음.")
        print("안녕히가세요")
        sys.exit()
        break
print()

# 품질인증여부 확인
dd = pd.read_csv('inspection.csv')

dd1 = dd.iloc[:,4] #품목
dd2 = dd.iloc[:,7] #총수입건수
dd3 = dd.iloc[:,10] #품질인증 부적합건수

list1 = [] #품목 종류를 저장할 리스트
for indata in dd1:
    indata = indata.replace("냉동", "") #원활한 품목검색을 위하여 공통 글 공백화
    indata = indata.replace("냉장", "")
    indata = indata.replace("활", "")
    list1.append(indata)

list2 = [] #총수입건수 저장 리스트
for indata in dd2:
    list2.append(indata)

list3 = [] #부적합건수 저장 리스트
for indata in dd3:
    list3.append(indata)

dict1 = dict(zip(list1, list2)) #딕셔너리화
dict2 = dict(zip(list1, list3))

if user in list1 :
    print("총 수입건수"   + str(dict1[user]))

    if dict2[user] != 0:
        print("품질인증 부적합건수"   + str(dict2[user]))
        print()
    else:
        print("품질인증 부적합건수 : 0")
        print()

#4. user가 원하면 관심있는 품목을 취급하는 위판장의 정보 제공
user_want =  input("위판장의 정보를 제공받으시겠습니까? (Y or N)").upper()
dfff = pd.read_csv('sale.csv') #수입 수산물 위판장 정보 제공 공공데이터

dfff1 = dfff.iloc[160:, 1] #위판장 정보 데이터에서 품목 종류, 160까지 같은 데이터라 시작점 160으로 설정.
#print(dfff1)
dfff2 = dfff.iloc[160:, 5] #위판장 정보 데이터에서 위판장 이름
#print(dfff2)

matrix4 = [] # 품목 이름을 저장할 리스트
for indata3 in dfff1:
    indata3 = indata3.replace("'","")
    matrix4.append(indata3)
#print(matrix4)

dfff3 = dfff1 + dfff2 #품목 이름과 위판장 df를 합치기
#print(dfff3)
matrix5 = [] # 품목 이름과 위판장을 저장할 리스트
for indata4 in dfff3:
    indata4 = indata4.replace("''",":")
    indata4 = indata4.replace("'","")
    #print(indata4)
    matrix5.append(indata4)
#print(matrix5)
set_matrix5 = list(set(matrix5)) #중복되는 품목별 위판장을 제거
#print(set_matrix5)

if user_want == 'Y':
    print("-----위판장 정보-----")
    if user in matrix4:
        for product in set_matrix5:
            product1 = product.split(":")
            if product1[0] == user:
                print(product1[1])
    else:
        print("위판정보없음.")

else:
    print("안녕히가세요.")

~~~

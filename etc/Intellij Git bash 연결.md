# Intellij Git bash 연결

로컬에서 shell file 실행 시 window 환경이어서 shell script실행이 안되는 문제 발생

=> terminal 환경을 git bash로 바꾸자!

## Settings 변경

1. Settings > terminal 검색
2. Tools > Terminal
3. Application Settings > Shell path
4. `"C:\Program Files\Git\bin\sh.exe" -login -i` 입력
5. IntelliJ restart

![image](https://user-images.githubusercontent.com/19357410/210466362-b8857ec6-6619-4d1c-b211-b77988abef37.png)

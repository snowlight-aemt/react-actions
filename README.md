## Github Actions

### 샘플 1
```yml
name: workflow name
on: github event
jobs:
  echo: # jobs 식별자
    runs-on: ubuntu-latest # CI OS 환경
    steps:                 # 작업 실행 단계
      - run: echo 'Hello, Github Actions~!' # shell command
```

### 샘플 2 (checkout@v3)
```yml
name: Workflow
on: push
jobs:
  checkout:
    runs-on: ubuntu-latest
    steps:
      - run: ls -al
      - uses: actions/checkout@v3  # 사용 actions 
        with:                      # uses 의 매개변수(입력 값) 을 전달 할 때 사용
          path: react-source       # checkout@v3 에서 path 매개변수 지원.
      - run: ls -al react-source
      - run: cat react-source/.github/workflows/checkout.yml
```

## Github Actions

### 샘플 1
```yml
name: workflow name
on: push # github event
jobs:
  echo: # jobs 식별자
    runs-on: ubuntu-latest # CI OS 환경
    steps:                 # 작업 실행 단계
      - run: echo 'Hello, Github Actions~!' # shell command
```

### 샘플 2
```yml
name: workflow name
on: push # github event
jobs:
  echo: # jobs 식별자
    runs-on: ubuntu-latest # CI OS 환경
    steps:                 # 작업 실행 단계
      - run: echo 'Hello, Github Actions~!' # shell command
  pwd:
    name: PWD
    runs-on: ubuntu-latest
    steps:
      - name: Print Working Directory
        run: pwd
```

### 샘플 3 (checkout@v3)
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

### 샘플 4 (env 환경 변수 )
* 환경 변수 사용
* if 문법
* 실제를 무시하고 다음 명령어를 실행한다.
  * continue-on-error: true
```yml
name: workflow name
on: push
jobs:
  foobar:
    runs-on: ubuntu-latest
    steps:
      - id: set-foo  # 1 단계 의 식별자 
        run: echo "foo=bar" >> $GITHUB_OUTPUT       # name=value
      - run: echo ${{ steps.set-foo.outputs.foo }}  # steps.<step_id>.outputs.foo | step_id : steps 의 단계 식별자
  calc:
    runs-on: ubuntu-latest
    steps:
      - id: num1  # 1 단계 의 식별자 
        run: echo "num=$(($RANDOM % 10 + 1))" >> $GITHUB_OUTPUT
      - id: num2  # 1 단계 의 식별자 
        run: echo "num=$(($RANDOM % 10 + 1))" >> $GITHUB_OUTPUT
      - run: echo $(( ${{ steps.num1.outputs.num }} + ${{ steps.num1.outputs.num }} ))
  zeroone:
    runs-on: ubuntu-latest
    steps:
      - id: num
        run: echo "num=$(($RANDOM % 2))" >> $GITHUB_OUTPUT
      - if: steps.num.outputs.num == 0
        run: echo 'zero'
      - if: steps.num.outputs.num == 1
        run: echo 'one'
  ignore:
    runs-on: ubuntu-latest
    steps:
      - id: fail
        continue-on-error: true
        run: exit 1
      - run: echo '[Error] 에러 발생'

```

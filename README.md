# ***`jq`*** 스크립트

## 1. JSON 샘플 파일

### 1.1 sample-01.json 파일

```json
{
  "whoami": {
    "company": "Red Hat",
    "user": "shadowman",
    "info": {
      "role": "sa",
      "language": "korean"
    }
  },
  "products": [
    {
      "openshift": {
        "os": "RHCOS",
        "type": "platform"
      }
    },
    {
      "odf": {
        "os": "RHCOS",
        "type": "storage"
      }
    },
    {
      "ansible": {
        "os": [
          "RHEL",
          "RHCOS"
        ],
        "type": "automation"
      }
    }
  ],
  "fruits": [
    {
      "name": "grape",
      "color": "violet"
    },
    {
      "name": "banana",
      "color": "yellow"
    },
    {
      "name": "apple",
      "color": "red"
    }
  ],
  "grade": [
    "plantinum",
    "gold",
    "silver",
    "bronze"
  ]
}
```
<br>
<br>

## 2. JSON 기본

### 2.1 JSON 출력

#### 2.1.1 *jq* 일반 출력

실행 명령어
```bash
echo '{"company":"Red Hat","user":"shadowman"}' | jq .
```

실행 결과 JSON 출력
```json
{
  "company": "Red Hat",
  "user": "shadowman"
}
```

#### 2.1.2 *jq* 컴팩트 출력

실행 명령어
```bash
echo '{"company":"Red Hat","user":"shadowman"}' | jq -c .
```

실행 결과 JSON 출력
```json
{"company":"Red Hat","user":"shadowman"}
```
<br>

### 2.2 JSON 파일 생성 및 파일 읽기

실행 명령어
```bash
echo '{"company":"Red Hat","user":"shadowman"}' | jq . > my-info.json
jq '.' my-info.json
```

실행 결과 JSON 출력
```json
{
  "company": "Red Hat",
  "user": "shadowman"
}
```
<br>

### 2.3 JSON 요소 출력

#### 2.3.1 *jq* 하위 요소 필터링 출력

실행 명령어
```bash
cat sample-01.json | jq '.whoami'
```

실행 결과 JSON 출력
```json
{
  "company": "Red Hat",
  "user": "shadowman",
  "info": {
    "role": "sa",
    "language": "korean"
  }
}
```

#### 2.3.2 *jq* 하위 요소 필터링 컴팩트 출력

실행 명령어
```bash
cat sample-01.json | jq -c '.whoami'
```

실행 결과 JSON 출력
```json
{"company":"Red Hat","user":"shadowman","info":{"role":"sa","language":"korean"}}
```

#### 2.3.3 선택 필드에서 하위 부분 삭제 후 출력

실행 명령어
```bash
cat sample-01.json | jq '.whoami | del(.info)'
```

실행 결과 JSON 출력
```json
{
  "company": "Red Hat",
  "user": "shadowman"
}
```

#### 2.3.4 하위 요소 필터링 및 Raw 스트링으로 출력

실행 명령어
```bash
cat sample-01.json | jq -r '.whoami' | jq '.user'
cat sample-01.json | jq -r '.whoami | .user'
cat sample-01.json | jq -r '.whoami.user'
```

실행 결과 JSON 출력
```json
shadowman
```
<br>

### 2.4 배열 요소 출력

#### 2.4.1 하위 요소의 배열 중 선택 출력

실행 명령어
```bash
cat sample-01.json | jq '.products[1]'
```

실행 결과 JSON 출력
```json
{
  "odf": {
    "os": "RHCOS",
    "type": "storage"
  }
}
```

#### 2.4.2 배열의 첫 번째 요소 출력

실행 명령어
```bash
cat sample-01.json | jq '.products[0]'
cat sample-01.json | jq '.products | first'
```

실행 결과 JSON 출력
```json
{
  "openshift": {
    "os": "RHCOS",
    "type": "platform"
  }
}
```

#### 2.4.3 배열의 마지막 요소 출력

실행 명령어
```bash
cat sample-01.json | jq '.products[-1]'
cat sample-01.json | jq '.products | last'
```

실행 결과 JSON 출력
```json
{
  "ansible": {
    "os": [
      "RHEL",
      "RHCOS"
    ],
    "type": "automation"
  }
}
```

#### 2.4.4 배열에서 선택 요소 출력

실행 명령어
```bash
cat sample-01.json | jq '.products[1, -1]'
```

실행 결과 JSON 출력
```json
{
  "odf": {
    "os": "RHCOS",
    "type": "storage"
  }
}
{
  "ansible": {
    "os": [
      "RHEL",
      "RHCOS"
    ],
    "type": "automation"
  }
}
```
<br>

### 2.5 JSON 하위 키 크기

#### 2.5.1 JSON 하위 객체 키 크기

실행 명령어
```bash
cat sample-01.json | jq '.whoami | length'
```
* 3을 반환

#### 2.5.3 JSON 하위 배열 크기

실행 명령어
```bash
cat sample-01.json| jq '.grade | length'
```
* 4를 반환

실행 명령어
```bash
cat sample-01.json| jq '.products | length'
```
* 3을 반환

<br>
<br>

## 3. JSON을 다른 JSON 형태로 추출

### 3.1 JSON 요소를 객체 형식으로 출력

#### 3.1.1 1개의 요소를 객체로 출력

실행 명령어
```bash
cat sample-01.json | jq '.whoami | {"user": .user}'
```

실행 결과 JSON 출력
```json
{
  "user": "shadowman"
}
```

#### 3.1.2 선택한 요소를 각각의 객체로 출력

실행 명령어
```bash
cat sample-01.json | jq '.whoami | {"user": .user}, {"role": .info.role}'
```

실행 결과 JSON 출력
```json
{
  "user": "shadowman"
}
{
  "role": "sa"
}
```

#### 3.1.3 선택한 요소를 1개의 객체로 출력

실행 명령어
```bash
cat sample-01.json | jq '.whoami | {"user": .user, "role": .info.role}'
```

실행 결과 JSON 출력
```json
{
  "user": "shadowman",
  "role": "sa"
}
```
<br>

### 3.2 선택한 요소를 배열로 출력

#### 3.2.1 1개의 요소를 배열로 출력

실행 명령어
```bash
cat sample-01.json | jq '.whoami | [.user]'
```

실행 결과 JSON 출력
```json
[
  "shadowman"
]
```

#### 3.2.2 선택한 요소를 각각의 배열로 출력

실행 명령어
```bash
 cat sample-01.json | jq '.whoami | [.user], [.info.role]'
```

실행 결과 JSON 출력
```json
[
  "shadowman"
]
[
  "sa"
]
```

#### 3.2.3 선택한 요소들을 하나의 배열로 출력

실행 명령어
```bash
cat sample-01.json | jq '.whoami | [.user, .info.role]'
```

실행 결과 JSON 출력
```json
[
  "shadowman",
  "sa"
]
```
<br>

### 3.3 JSON 요소를 다양한 형식으로 출력

#### 3.3.1 객체를 가진 배열 출력

실행 명령어
```bash
cat sample-01.json | jq '.whoami | [{"user": .user}, {"role": .info.role}]'
```

실행 결과 JSON 출력
```json
[
  {
    "user": "shadowman"
  },
  {
    "role": "sa"
  }
]
```

#### 3.3.2 선택한 객체 출력

실행 명령어
```bash
cat sample-01.json | jq -r '.whoami | .user, .info.role'
```

실행 결과 JSON 출력
```json
shadowman
sa
```

#### 3.3.3 샌택한 객체를 1줄로 출력

실행 명령어
```bash
cat sample-01.json | jq -r '.whoami | "\(.user), \(.info.role)"'
```

실행 결과 JSON 출력
```json
shadowman, sa
```

#### 3.3.4 선택한 객체를 형식으로 문자열로 출력

실행 명령어
```bash
cat sample-01.json | jq -r '.whoami | "user=\(.user), role=\(.info.role)"'
```

실행 결과 JSON 출력
```json
user=shadowman, role=sa
```

#### 3.3.5 선택한 객체를 다른 형식의 배열로 출력

실행 명령어
```bash
cat sample-01.json | jq -r '.whoami | ["user=\(.user)", "role=\(.info.role)"]'
```

실행 결과 JSON 출력
```json
[
  "user=shadowman",
  "role=sa"
]
```
<br>
<br>

## 4. JSON 요소 작업

### 4.1 JSON 요소 변경

#### 4.1.1 하위 오브젝트 변경

실행 명령어
```bash
cat sample-01.json | jq '.whoami | .info.role = "Account SA"'
```

실행 결과 JSON 출력
```json
{
  "company": "Red Hat",
  "user": "shadowman",
  "info": {
    "role": "Account SA",
    "language": "korean"
  }
}
```

#### 4.1.2 하위 배열 변경 (1/2)

실행 명령어
```bash
cat sample-01.json | jq '.grade[-1] = "diamond" | .grade'
cat sample-01.json | jq '.grade | last = "diamond"'
```

실행 결과 JSON 출력
```json
[
  "plantinum",
  "gold",
  "silver",
  "diamond"
]
```

#### 4.1.3 하위 배열 변경 (2/2)

실행 명령어
```bash
cat sample-01.json| jq '.grade[1] = "ruby" | .grade'
cat sample-01.json | jq '.grade | .[1] = "ruby"'
```

실행 결과 JSON 출력
```json
[
  "plantinum",
  "ruby",
  "silver",
  "bronze"
]
```
<br>

### 4.2 JSON 요소 추가

#### 4.2.1 오브젝트에 "email" 요소 추가

실행 명령어
```bash
cat sample-01.json | jq '.whoami | .info += {"email": "shadowman@redhat.lab"}'
```

실행 결과 JSON 출력
```json
{
  "company": "Red Hat",
  "user": "shadowman",
  "info": {
    "role": "sa",
    "language": "korean",
    "email": "shadowman@redhat.lab"
  }
}
```

#### 4.2.2 배열 "grade"에 요소 추가

실행 명령어
```bash
cat sample-01.json | jq '.grade += ["ruby"] | .grade'
```

실행 결과 JSON 출력
```json
[
  "plantinum",
  "gold",
  "silver",
  "bronze",
  "ruby"
]
```
<br>

### 4.3 JSON 요소 삭제

#### 4.3.1 하위 오브젝트 삭제

실행 명령어
```bash
cat sample-01.json | jq '.whoami | del(.info.role)'
```

실행 결과 JSON 출력
```json
{
  "company": "Red Hat",
  "user": "shadowman",
  "info": {
    "language": "korean"
  }
}
```

#### 4.3.2 하위 배열 요소 삭제 (1/2)

실행 명령어
```bash
cat sample-01.json | jq 'del(.grade[1]) | .grade'
cat sample-01.json | jq '.grade | del(.[1])'
```

실행 결과 JSON 출력
```json
[
  "plantinum",
  "silver",
  "bronze"
]
```

#### 4.3.3 하위 배열 요소 삭제 (2/2)

실행 명령어
```bash
cat sample-01.json| jq 'del(.grade[]|select(contains("gold")))|.grade'
cat sample-01.json| jq '.grade | del(.[]|select(contains("gold")))'
```

실행 결과 JSON 출력
```json
[
  "plantinum",
  "silver",
  "bronze"
]
```
<br>

### 4.4 JSON 키 검색

#### 4.4.1 존재하는 키 (*role*) 검색

실행 명령어
```bash
cat sample-01.json| jq -e 'any(paths[-1]; .== "role")'; echo $?
```
* 키가 존재하며, 0(true)을 반환

#### 4.4.2 존재하지 않는 키 (*email*) 검색 

실행 명령어
```bash
cat sample-01.json| jq -e 'any(paths[-1]; .== "email")'; echo $?
```
* 키가 존재하지 않으며, 1(false)를 반환

#### 4.4.3 키(*role*)의 경로를 배열로 표시

실행 명령어
```bash
cat sample-01.json| jq 'paths | select(.[-1] == "role")'
```

실행 결과 JSON 출력
```json
[
  "whoami",
  "info",
  "role"
]
```

#### 4.4.4 모든 키의 경로와 값을 표시

실행 명령어
```bash
cat sample-01.json | jq 'paths(scalars) as $p | [([$p[]|tostring]|join(".")), (getpath($p)|tojson)] | join(": ")'
```

실행 결과 JSON 출력
```json
"whoami.company: \"Red Hat\""
"whoami.user: \"shadowman\""
"whoami.info.role: \"sa\""
"whoami.info.language: \"korean\""
"products.0.openshift.os: \"RHCOS\""
"products.0.openshift.type: \"platform\""
"products.1.odf.os: \"RHCOS\""
"products.1.odf.type: \"storage\""
"products.2.ansible.os.0: \"RHEL\""
"products.2.ansible.os.1: \"RHCOS\""
"products.2.ansible.type: \"automation\""
"fruits.0.name: \"grape\""
"fruits.0.color: \"violet\""
"fruits.1.name: \"banana\""
"fruits.1.color: \"yellow\""
"fruits.2.name: \"apple\""
"fruits.2.color: \"red\""
"grade.0: \"plantinum\""
"grade.1: \"gold\""
"grade.2: \"silver\""
"grade.3: \"bronze\""
```
<br>
<br>

## 5. JSON 파일 변환

### 5.1 JSON을 YAML 변환

실행 명령어
```bash
cat sample-01.json | jq '.whoami' | yq -y
```

실행 결과 YAML 출력
```yaml
{
  "company": "Red Hat",
  "user": "shadowman",
  "info": {
    "role": "sa",
    "language": "korean"
  }
}
```

### 5.2 YAML을 JSON으로 변환

실행 명령어
```bash
cat sample-01.json | jq '.whoami' | yq -y | yq 
```

실행 결과 JSON 출력
```json
{
  "company": "Red Hat",
  "user": "shadowman",
  "info": {
    "role": "sa",
    "language": "korean"
  }
}
```
<br>
<br>

## 99. JSON 출력 명려어

### 99.1 IP 정보 확인

#### 99.1.1 *ip* 명령어

실행 명령어
```bash
ip -j a
```
<br>

### 99.2 블록 디바이스 확인

#### 99.2.1 *lsblk* 명령어

실행 명령어
```bash
lsblk --json
```
<br>

### 99.3 파일시스템 확인

#### 99.3.1 *findmnt* 명령어

실행 명령어
```bash
findmnt --json
```
<br>

### 99.4 오픈스택

<br>

### 99.5 포드맨 (podman)

<br>

### 99.6 오픈시프트


<br>
<br>

실행 명령어
```bash

```

실행 결과 JSON 출력
```json

```

<br>
<br>

```
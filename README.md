## Description

- Gitlab-runner for CI/CD using docker-compose.
- step1 : runner 실행 후 
  - step2 : 첫 실행시 ssh 접속을 위한 파일을 생성한다.
  - step3 : runner를 등록한다. (등록 명령을 수행하면 config.toml 파일에 기록된다.)
    - url과 token은 아래 경로에서 찾음.
    - [그룹] > Settings > CI/CD > Runners > Group runners
  - CI/CD Jobs 실행할때 SSH 오류가 발생하면 'disable_strict_host_key_checking = true'를 '[runner.ssh]' 항목에 추가

## Step 1. Run
```bash
$ docker compose up -d 
```

## Step 2. Init (처음 설치 시)
```bash
# make dir for known_hosts file
$ docker compose exec -it -u 0 runner bash -c "mkdir /root/.ssh"

# regist dev-server and prod-server
$ docker compose exec -it -u 0 runner bash -c "ssh-keyscan -t rsa 192.168.35.11 >> ~/.ssh/known_hosts"
$ docker compose exec -it -u 0 runner bash -c "ssh-keyscan -t rsa 192.168.35.14 >> ~/.ssh/known_hosts"
```

## Step 3. Regist group runner
```bash
$ docker run --rm -it -v ${PWD}/conf.d/runner:/etc/gitlab-runner gitlab/gitlab-runner register
```
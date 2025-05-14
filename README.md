
# 🚀 Spring Boot + Docker + MySQL N-Tier Application

이 프로젝트는 Spring Boot 백엔드 애플리케이션을 Docker 기반으로 배포하며, MySQL 데이터베이스와 연동되는 N-Tier 구조의 예제입니다.

## 📁 프로젝트 구성
```markdown
├── Dockerfile             # Spring Boot 실행용 Docker 이미지 정의
├── app.sql                # 초기 DB 테이블 및 데이터 정의
├── mvnw / pom.xml / src/  # Spring Boot 프로젝트
```
---

## 🎯 목표

- GitHub → 서버로 코드 배포
- Maven으로 jar 빌드
- Docker로 백엔드 + DB 환경 실행

---

## 📦 실행 방법 (서버/로컬 환경에서)

### 1️⃣ 프로젝트 클론

```bash
git clone https://github.com/your-id/your-repo.git
cd your-repo
````

### 2️⃣ Maven 빌드

```bash
./mvnw clean package
```

> 🔸 `target/app-0.0.1-SNAPSHOT.jar` 생성됨

---

### 3️⃣ 인프라 디렉터리 구성

```bash
mkdir -p ~/infra/backend
cp target/app-0.0.1-SNAPSHOT.jar Dockerfile ~/infra/backend/
cd ~/infra/backend
```

---

### 4️⃣ Docker 네트워크 생성

```bash
docker network create app-network
```

---

### 5️⃣ MySQL 컨테이너 실행

```bash
mkdir -p /home/ubuntu/data/mysql

docker run --name mysql-server -p 3306:3306 \
--network app-network \
-v /home/ubuntu/data/mysql:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-e MYSQL_DATABASE=app \
-e MYSQL_USER=app \
-e MYSQL_PASSWORD=app \
-d mysql:latest
```

#### 🔹 DB 초기화

```bash
docker cp ../app.sql mysql-server:/app.sql
docker exec -it mysql-server bash
mysql -u root -p app < /app.sql
exit
```

---

### 6️⃣ 백엔드 Docker 이미지 빌드 & 실행

```bash
docker build -t backend-server .
docker run -d -p 8080:8080 \
--name backend-server \
--network app-network \
backend-server
```

---

### 7️⃣ 접속 테스트

웹 브라우저 또는 curl로 확인:

```
http://localhost:8080
```

또는

```
http://<서버 IP>:8080
```

---

## 💡 참고 사항

* `.jar` 파일은 GitHub에 포함되지 않음 → 서버에서 직접 빌드
* DB 연결 주소는 다음과 같이 설정되어야 합니다:

```properties
spring.datasource.url=jdbc:mysql://mysql-server:3306/app?serverTimezone=Asia/Seoul
```

---

## 🧑‍💻 개발 환경

* Java 17
* Spring Boot 3.x
* Maven Wrapper (`./mvnw`)
* Docker
* MySQL (컨테이너 기반)

---

## 📜 라이선스

MIT License

---

## 🙋 문의

* 작성자: 당신의 이름 또는 팀
* GitHub: [@your-id](https://github.com/your-id)

```

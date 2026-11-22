# Bài 1: Sửa lỗi cấu hình chia sẻ Docker Socket (DooD)

## 1. Nguyên nhân gốc rễ (Root Cause)
Lỗi `Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?` xảy ra vì container Self-hosted Runner chạy cô lập, chưa được chia sẻ socket Unix `/var/run/docker.sock` từ Docker Daemon của máy host (cơ chế Docker-outside-of-Docker / DooD).

## 2. Cách khắc phục (`docker-compose.yml`)
Mount trực tiếp Docker socket và thư mục làm việc tạm từ host vào container Runner:
```yaml
volumes:
  - /var/run/docker.sock:/var/run/docker.sock
  - /tmp/runner:/tmp/runner
```

## 3. Lệnh khởi chạy Runner
1. Lấy `RUNNER_TOKEN` tại `Settings > Actions > Runners > New self-hosted runner`.
2. Chạy lệnh trong WSL:
```bash
RUNNER_TOKEN="<token>" docker compose up -d
```

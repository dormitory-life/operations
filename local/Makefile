AUTH_SVC_PATH = $(CURDIR)/auth
CORE_SVC_PATH = $(CURDIR)/core
GATEWAY_SVC_PATH = $(CURDIR)/gateway
DB_COMPOSE_PATH = $(CURDIR)
STORAGE_COMPOSE_PATH = $(CURDIR)
BROKER_COMPOSE_PATH = $(CURDIR)
COMPOSE_PATH = $(CURDIR)
FRONTEND_PATH = $(CURDIR)/frontend
PID_DIR = .pids

$(shell mkdir -p $(PID_DIR))

.PHONY: local, local-build, local-run, db-up, db-down, broker-up, broker-down, infra-build, infra-up, infra-down, svc-up, svc-down, project-up, project-down, front-run, front-stop, networks-up, networks-down

local: local-build local-run

db-up:
	@echo "DB up..."
	@cd $(DB_COMPOSE_PATH) && docker compose up db -d
	@echo "waiting for db..."
	@for i in 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30; do \
		if docker exec dormitory-life-db-1 pg_isready -U user >/dev/null 2>&1; then \
			echo "DB is ready..."; \
			break; \
		fi; \
		echo "   waiting for db ($$i/30)"; \
		sleep 1; \
		if [ $$i -eq 30 ]; then \
			echo "db start error since 30s"; \
			exit 1; \
		fi; \
	done
	@sleep 2

db-container-up:
	@echo "DB up..."
	@cd $(DB_COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml up db -d
	@echo "waiting for db..."
	@for i in 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30; do \
		if docker exec dormitory-life-db-1 pg_isready -U user >/dev/null 2>&1; then \
			echo "DB is ready..."; \
			break; \
		fi; \
		echo "   waiting for db ($$i/30)"; \
		sleep 1; \
		if [ $$i -eq 30 ]; then \
			echo "db start error since 30s"; \
			exit 1; \
		fi; \
	done
	@sleep 2


db-down:
	@echo "DB down"
	@cd $(DB_COMPOSE_PATH) && docker compose down db -v

db-container-down:
	@echo "DB down"
	@cd $(DB_COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml down db -v

local-build:
	@echo "Building gateway svc..."
	@cd $(GATEWAY_SVC_PATH) && go build -o .bin/main cmd/main.go

	@echo "Building auth svc..."
	@cd $(AUTH_SVC_PATH) && go build -o .bin/main cmd/main.go

	@echo "Building core svc..."
	@cd $(CORE_SVC_PATH) && go build -o .bin/main cmd/main.go

local-run: infra-up
	@echo "Starting auth svc..."
	@cd $(AUTH_SVC_PATH) && go run cmd/main.go configs/local.yaml &

	@sleep 5s

	@echo "Starting core svc..."
	@cd $(CORE_SVC_PATH) && go run cmd/main.go configs/local.yaml &

	@echo "Starting gateway svc..."
	@cd $(GATEWAY_SVC_PATH) && go run cmd/main.go configs/local.yaml &

storage-up:
	@echo "storage up"
	@mkdir -p ./data/minio
	@cd $(STORAGE_COMPOSE_PATH) && docker compose up storage -d
	@echo "Waiting for MinIO to be ready..."
	@for i in 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30; do \
		if curl -s http://localhost:9000/minio/health/live >/dev/null 2>&1; then \
			echo "MinIO is ready..."; \
			echo "Web UI: http://localhost:9001"; \
			echo "S3 API: http://localhost:9000"; \
			echo "Credentials: minioadmin/minioadmin"; \
			break; \
		fi; \
		echo "   waiting for MinIO ($$i/30)"; \
		sleep 1; \
		if [ $$i -eq 30 ]; then \
			echo "MinIO start error (timeout 30s)"; \
			echo "Checking MinIO logs..."; \
			cd $(STORAGE_COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml logs minio; \
			exit 1; \
		fi; \
	done
	@sleep 2

storage-container-up:
	@echo "storage up"
	@mkdir -p ./data/minio
	@cd $(STORAGE_COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml up storage -d
	@echo "Waiting for MinIO to be ready..."
	@for i in 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30; do \
		if curl -s http://localhost:9000/minio/health/live >/dev/null 2>&1; then \
			echo "MinIO is ready..."; \
			echo "Web UI: http://localhost:9001"; \
			echo "S3 API: http://localhost:9000"; \
			echo "Credentials: minioadmin/minioadmin"; \
			break; \
		fi; \
		echo "   waiting for MinIO ($$i/30)"; \
		sleep 1; \
		if [ $$i -eq 30 ]; then \
			echo "MinIO start error (timeout 30s)"; \
			echo "Checking MinIO logs..."; \
			cd $(STORAGE_COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml logs minio; \
			exit 1; \
		fi; \
	done
	@sleep 2

storage-down:
	@echo "storage down"
	@cd $(STORAGE_COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml down -v

storage-container-down:
	@echo "storage down"
	@cd $(STORAGE_COMPOSE_PATH) && docker compose down storage -v

broker-up:
	@echo "broker up"
	@cd $(BROKER_COMPOSE_PATH) && docker compose up broker -d
	@echo "Waiting for RabbitMQ to be ready..."
	@for i in 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30; do \
		if cd $(BROKER_COMPOSE_PATH) && docker compose exec -T broker rabbitmq-diagnostics -q ping >/dev/null 2>&1; then \
			echo "RabbitMQ is ready..."; \
			break; \
		fi; \
		echo "   waiting for RabbitMQ ($$i/30)"; \
		sleep 1; \
		if [ $$i -eq 30 ]; then \
			echo "RabbitMQ start error (timeout 30s)"; \
			cd $(BROKER_COMPOSE_PATH) && docker compose logs broker; \
			exit 1; \
		fi; \
	done
	@sleep 2

broker-container-up:
	@echo "broker up"
	@cd $(BROKER_COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml up broker -d
	@echo "Waiting for RabbitMQ to be ready..."
	@for i in 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30; do \
		if cd $(BROKER_COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml exec -T broker rabbitmq-diagnostics -q ping >/dev/null 2>&1; then \
			echo "RabbitMQ is ready..."; \
			break; \
		fi; \
		echo "   waiting for RabbitMQ ($$i/30)"; \
		sleep 1; \
		if [ $$i -eq 30 ]; then \
			echo "RabbitMQ start error (timeout 30s)"; \
			cd $(BROKER_COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml logs broker; \
			exit 1; \
		fi; \
	done
	@sleep 2

broker-down:
	@echo "broker up"
	@cd $(BROKER_COMPOSE_PATH) && docker compose down broker -v

broker-container-down:
	@echo "broker up"
	@cd $(BROKER_COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml down -v


cache-up:
	@echo "cache up"
	@cd $(COMPOSE_PATH) && docker compose up cache -d
	@echo "Waiting for Redis to be ready..."
	@for i in 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30; do \
		if cd $(COMPOSE_PATH) && docker compose exec -T cache redis-cli -a admin ping | grep PONG >/dev/null 2>&1; then \
			echo "Redis is ready..."; \
			break; \
		fi; \
		echo "   waiting for Redis ($$i/30)"; \
		sleep 1; \
		if [ $$i -eq 30 ]; then \
			echo "Redis start error (timeout 30s)"; \
			cd $(COMPOSE_PATH) && docker compose logs cache; \
			exit 1; \
		fi; \
	done
	@sleep 2


cache-container-up:
	@echo "cache up"
	@cd $(COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml up cache -d
	@echo "Waiting for Redis to be ready..."
	@for i in 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30; do \
		if cd $(COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml exec -T cache redis-cli -a admin ping | grep PONG >/dev/null 2>&1; then \
			echo "Redis is ready..."; \
			break; \
		fi; \
		echo "   waiting for Redis ($$i/30)"; \
		sleep 1; \
		if [ $$i -eq 30 ]; then \
			echo "Redis start error (timeout 30s)"; \
			cd $(COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml logs cache; \
			exit 1; \
		fi; \
	done
	@sleep 2

cache-down:
	@echo "cache down"
	@cd $(COMPOSE_PATH) && docker compose down cache -v

cache-container-down:
	@echo "cache down"
	@cd $(COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml down cache -v

infra-build:
	@echo "infra build"
	@cd $(COMPOSE_PATH) && docker compose build --no-cache

infra-container-build:
	@echo "infra build"
	@cd $(COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml build --no-cache

infra-up: storage-up db-up broker-up cache-up

infra-container-up: storage-container-up db-container-up broker-container-up cache-container-up

infra-down:
	@echo "infra down"
	@cd $(COMPOSE_PATH) && docker compose down

infra-container-down:
	@echo "infra down"
	@cd $(COMPOSE_PATH) && docker compose -f docker-compose-infra.yaml down

local-stop:
	@echo "Stopping processes on ports..."
	@for p in 8080 8081 8082; do \
		if lsof -ti:$$p >/dev/null 2>&1; then \
			echo "Port $$p is occupied. Killing process..."; \
			kill -9 $$(lsof -ti:$$p) 2>/dev/null || true; \
		else \
			echo "Port $$p is free"; \
		fi; \
	done
	@echo "All processes are stopped"

svc-up: infra-container-up
	@echo "Starting Auth..."
	@cd $(AUTH_SVC_PATH) && docker compose -f docker-compose-svc.yaml up --build -d

	@echo "Starting Core..."
	@cd $(CORE_SVC_PATH) && docker compose -f docker-compose-svc.yaml up --build -d

	@echo "Starting Gateway..."
	@cd $(GATEWAY_SVC_PATH) && docker compose -f docker-compose-svc.yaml up --build -d

svc-down:
	@echo "Stopping Gateway..."
	@cd $(GATEWAY_SVC_PATH) && docker compose -f docker-compose-svc.yaml down -v

	@echo "Stopping Auth..."
	@cd $(AUTH_SVC_PATH) && docker compose -f docker-compose-svc.yaml down -v

	@echo "Stopping Core..."
	@cd $(CORE_SVC_PATH) && docker compose -f docker-compose-svc.yaml down -v

networks-up:
	@echo "Creating networks..."
	@docker network inspect core-gateway >/dev/null 2>&1 || docker network create core-gateway
	@docker network inspect core-s3 >/dev/null 2>&1 || docker network create core-s3
	@docker network inspect core-broker >/dev/null 2>&1 || docker network create core-broker
	@docker network inspect core-db >/dev/null 2>&1 || docker network create core-db
	@docker network inspect auth-db >/dev/null 2>&1 || docker network create auth-db
	@docker network inspect auth-gateway >/dev/null 2>&1 || docker network create auth-gateway
	@docker network inspect auth-core >/dev/null 2>&1 || docker network create auth-core
	@docker network inspect core-cache >/dev/null 2>&1 || docker network create core-cache
	@echo "Networks created"

networks-down:
	@echo "Removing networks..."
	@docker network rm core-gateway auth-gateway auth-core core-db auth-db core-s3 core-broker core-cache 2>/dev/null || true
	@echo "Networks removed"

front-run:
	@echo "Starting frontend..."
	@cd $(FRONTEND_PATH) && npm run dev > /tmp/frontend.log 2>&1 & echo $$! > $(PID_DIR)/frontend.pid
	@echo "Frontend started with PID: $$(cat $(PID_DIR)/frontend.pid)"
	@sleep 2
	@echo "Frontend logs: tail -f /tmp/frontend.log"

front-stop:
	@echo "Stopping frontend..."
	@if [ -f $(PID_DIR)/frontend.pid ]; then \
		kill -TERM $$(cat $(PID_DIR)/frontend.pid) 2>/dev/null || true; \
		rm -f $(PID_DIR)/frontend.pid; \
	fi
	@echo "Frontend stopped"

project-up: infra-build local-run
	@$(MAKE) front-run
	@echo ""
	@echo "=== ALL SERVICES STARTED ==="

project-down: local-stop infra-down front-stop 
	@echo "=== ALL SERVICES STOPPED ==="

project-container-up: networks-up infra-container-build svc-up
	@$(MAKE) front-run
	@echo ""
	@echo "=== ALL SERVICES STARTED ==="

project-container-down: svc-down infra-container-down networks-down front-stop 
	@echo "=== ALL SERVICES STOPPED ==="
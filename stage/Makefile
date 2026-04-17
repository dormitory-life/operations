COMPOSE=docker compose

.PHONY: up down logs ps pull restart clean

up:
	$(COMPOSE) up -d

down:
	$(COMPOSE) down

logs:
	$(COMPOSE) logs -f

ps:
	$(COMPOSE) ps

pull:
	$(COMPOSE) pull

restart:
	$(COMPOSE) restart

clean:
	$(COMPOSE) down -v --remove-orphans
## Usage:
To run "shop-demo" project you can use docker with compose file:

```
version: '3.8'

services:
  db:
    image: postgres
    container_name: shop_demo_db
    restart: always
    environment:
      POSTGRES_USER: "dbadmin"
      POSTGRES_PASSWORD: "dbadmin"
      POSTGRES_DB: "shop-demo"
    ports:
      - 5435:5432
    volumes:
      - db_data:/var/lib/postgresql/data

  shop-demo-server:
    image: vfomin/shop-demo-server:prod
    container_name: "shop_demo_server"
    restart: always
    environment:
      DB_NAME: "shop-demo"
      WAIT_HOSTS: db:5432
    ports:
      - 3001:3001
    depends_on:
      - db

  shop-demo-client:
    # Image tag name describes what state manager is used.
    # There are five possible tags:
    # - react_use_state (by default)
    # - redux
    # - mobx
    # - mobx_state_tree
    # - react_query
    image: vfomin/shop-demo-client:react_use_state
    container_name: shop_demo_client
    restart: always
    ports:
      - 3000:80
    depends_on:
      - shop-demo-server

volumes:
  db_data:
```

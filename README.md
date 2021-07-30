# Sequelize Cheatsheet
Sequelize Command Line Interface, and Migration, Model Cheatsheet


## Sequlize Command Line Interface

### Initialize Sequlize

```bash
sequelize init
```

### Create Database

```bash
sequelize db:create
```

### Drop Database
```bash
sequelize db:drop
```

### Create Model
```bash
sequelize model:generate --name User --attributes firstName:string,lastName:string,email:string
```

### Migration
```bash
sequelize db:migrate
```

### Generate Seeder
```bash
sequelize seed:generate --name demo-user
```

### Running Seeds
```bash
sequelize db:seed:all
```


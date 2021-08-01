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

### Relations
![image](https://user-images.githubusercontent.com/10147928/127766066-442fc354-3968-4f3c-8d23-997271000ed2.png)
#### Country Migration
```javascript
'use strict'
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('Countries', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      name: {
        type: Sequelize.STRING
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE,
        defaultValue: Sequelize.literal('CURRENT_TIMESTAMP()')
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE,
        defaultValue: Sequelize.literal('CURRENT_TIMESTAMP() ON UPDATE CURRENT_TIMESTAMP()')
      }
    })
  },
  down: async (queryInterface, Sequelize) => {
    await queryInterface.dropTable('Countries')
  }
}
```

#### Country Model
```javascript
'use strict'
const {
  Model
} = require('sequelize')
module.exports = (sequelize, DataTypes) => {
  class Country extends Model {
    static associate (models) {
      this.hasOne(models.CapitalCity)
    }
  };
  Country.init({
    name: DataTypes.STRING
  }, {
    sequelize,
    modelName: 'Country'
  })
  return Country
}
```
#### Capital City Migration
```javascript
'use strict'
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('CapitalCities', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      name: {
        type: Sequelize.STRING
      },
      // Foreign Key
      countryId: {
        type: Sequelize.INTEGER,
        references: {
          model: 'Countries',
          key: 'id'
        }
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE,
        defaultValue: Sequelize.literal('CURRENT_TIMESTAMP()')
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE,
        defaultValue: Sequelize.literal('CURRENT_TIMESTAMP() ON UPDATE CURRENT_TIMESTAMP()')
      }
    })
  },
  down: async (queryInterface, Sequelize) => {
    await queryInterface.dropTable('CapitalCities')
  }
}
```
#### Capital City Model
```javascript
'use strict'
const {
  Model
} = require('sequelize')
module.exports = (sequelize, DataTypes) => {
  class CapitalCity extends Model {
    static associate (models) {
      this.belongsTo(models.Country, {
      // Foreign Key
        foreignKey: {
          name: 'countryId'
        }
      })
    }
  };
  CapitalCity.init({
    name: DataTypes.STRING,
    countryId: DataTypes.INTEGER //Foreign Key
  }, {
    sequelize,
    modelName: 'CapitalCity'
  })
  return CapitalCity
}
```


Installation:
=============
    
    npm install https://github.com/shstefanov/infrastructure-mysql.git

Configuration:
==============

  In "data" structure configuration add:

  - In engines array: 

    "engines": [ "infrastructure-mysql/engine" ]

  - In loaders the built-in data loader

    "loaders": [ "data" ]

  - In libs:

    "libs":{
      "MysqlLayer":    "infrastructure-mysql/MysqlLayer"
    }

  - In "config":

    "config": {
      "mysql": {
        "host"     : "localhost",
        "user"     : "user_name",
        "password" : "user_password",
        "database" : "db_name"
      }      
    }


The MysqlLayer class
====================

  In folder, where datalayers are make file:

    module.exports = function(){
      var env     = this;
      return env.lib.MysqlLayer.extend("MysqlModel", {
        tableName: "models",
        primaryKey: "model_id",
        
        fields: {  // There is no role for fields values for now. The keys are used to filter given objects when creating or updating data
          model_id: "INT NOT NULL AUTO_INCREMENT PRIMARY KEY",
          field_a:  "varchar(10)",
          field_b:  "int"
        }
      });
    };

  For now it will work only with existing tables, so make sure the table exists in the database


Queries:
========

  ---
    // Create a record
    env.i.do("data.Blueprint.create", {
      path: "test.path",
      field_a:333,
      name: "Some name",
    }, function(err, record){
      console.log("data.Blueprint.create", record);
    });
  
  ---

    // Find all records
    env.i.do("data.Blueprint.find", function(err, records){
      console.log("data.Blueprint.find", records);
    });

  ---

    // Find one record by id
    env.i.do("data.Blueprint.find", 5, function(err, record){
      console.log("????", record);
    });

  ---

    // Find multiple records by array of ids
    env.i.do("data.Blueprint.find", [5,6,7], function(err, records){
      console.log("data.Blueprint.find", records);
    });

  ---

    // Find multiple records by condition
    env.i.do("data.Blueprint.find", {
      name: ["NOT IN", '1434118856493', 1434118849186 ], // 1434118849186 || [ 1434118849186, 1434118849186 ] || [">=", 1434118849186, 1434118849186 ]
      path: 'test.path'
    }, function(err, records){
      console.log("data.Blueprint.find", records);
    });

  ---

    // Find multiple records by condition and some options
    env.i.do("data.Blueprint.find", {
      name: [">", 0], // 1434118849186 || [ 1434118849186, 1434118849186 ] || [">=", 1434118849186, 1434118849186 ]
      path: 'test.path'
    }, {
      limit: [3, 3],
      order: ["name", "DESC"]
    }, function(err, records){
      console.log("data.Blueprint.find", records);
    });

  ---

    // Update record
    env.i.do("data.Blueprint.update", {
      // if primaryKey provided - only one record will be updated
      name: [">", 0], // varriants: 1434118849186 || [ 1434118849186, 1434118849186 ] || [">=", 1434118849186, 1434118849186 ]
      path: 'test.path'
    }, {
      // where: { ... } - if omitted, all records will be updated, otherwise where clause will be generated
      limit: [3, 3],
      order: ["name", "DESC"]
    }, function(err, result){
      console.log("data.Blueprint.update", result);
    });
  
  ---

    // Delete all record
    env.i.do("data.Blueprint.delete", function(err, result){
      console.log("data.Blueprint.delete", result);
    });

    // Delete one record by id
    env.i.do("data.Blueprint.delete", 15, function(err, result){
      console.log("data.Blueprint.delete", result);
    });

    // Delete with where condition
    env.i.do("data.Blueprint.delete", {field: value, otrherField: ["Between"]}, function(err, result){
      console.log("data.Blueprint.delete", result);
    });
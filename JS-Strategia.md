#### JavaScript

   База - ние!
   Id - avtomat!

   *********

1. Пускаме XAMPP - като admin и HEIDI:

   *********

2. Webstorm - Open

   *********

3. npm install
   Важно!!!
   Стартираме проекта:
   Edit Configuration:
   Node.js
   Javascript file: bin\www

   *********

4. Базата се създава в HEIDI - от config.js

   *********

5. TODO: Shift Shift TODO

   *********

6. Entity:
   Не създаваме id!
   
   DataTypes in Sequelize:
   http://docs.sequelizejs.com/manual/tutorial/models-definition.html
   
   *** SEQUELIZE Manual:
http://docs.sequelizejs.com/manual/tutorial/models-definition.html

*** Entity Project (id не се прави - то си се създава от sequelize):
sequelize.define('Project'... дефинира модела ''Project

******


Sequelize.DOUBLE


------

ENTITY:

-------

Rider:

-------

const Sequelize = require('sequelize');

module.exports = function (sequelize) {
    let Project = sequelize.define('Project', {
        title: {
            type: Sequelize.TEXT,
            allowNull: false,
        },
        description:{
            type: Sequelize.TEXT,
            allowNull: false,
        },
        budget:{
            type: Sequelize.INTEGER,
            allowNull: false,
        }
    },{
        timestamps:false
    });

    return Project;
};


------

Shopping List

------

const Sequelize = require('sequelize');

module.exports = function(sequelize){

    const Product = sequelize.define("Product", {
            priority: {
                type: Sequelize.INTEGER,
                required: true,
                allowNull: false
            },
            name: {
                type: Sequelize.TEXT,
                required: true,
                allowNull: false
            },
            quantity: {
                type: Sequelize.INTEGER,
                required: true,
                allowNull: false
            },
            status: {
                type: Sequelize.TEXT,
                required: true,
                allowNull: false
            }
    }, {
        timestamps: false
    });

    return Product;
};


-------

   *********
   
7. Controller - Alt + Enter и сменяме на ECMA Script 6
   Преписваме, като си гледаме какво се вика горе в контролера и какво има във view-tata
   
   !!! Важно: Разглеждаме си формите, за да видим дали се викат като project[priority] или само като 'priority'.
   
   Правило:
   ако във файла:
   project-rider/group2/JavaScript/views/project/create.hbs
   имаме name="project[title]" 
   тогава ползваме: let args = req.body.project;

   иначе: ако имаме: name="director"
   let body = req.body;

   Правило:
   ако във файла:
   project-rider/group2/JavaScript/views/project/create.hbs
   имаме name="project[title]" 
   тогава ползваме: let args = req.body.project;

   иначе: ако имаме: name="director"
   let body = req.body;
   
   -------
   za editGet: 
   a) product.dataValues - в случай, че се викат единични "priority", "quantity" t.n vyv formata za edit
   -
   editGet: (req, res) => {
		let id = req.params.id;

		Product.findById(id).then(product => {
            res.render("product/edit", product.dataValues);
		})

	},
    
    -
    b) { "project":project } - в случай, че се викат единични project[priority], project[quantity] t.n vyv formata za edit
    editGet: (req, res) => {
        let id = req.params.id;

        Project.findById(id).then(project => {
            res.render("project/edit", {"project":project});
        })
    },
    


------

Rider:

(vtori variant:)index: (req, res) => {
        let projects = Project.findAll().then(projects=>{
            res.render("project/index", {"projects": projects})
        });

---------

const Project = require('../models').Project;

module.exports = {
    index: (req, res) => {
        //nameri vsichki projects
        //posle gi podaj kato stojnost
        //renderiraj tova view i kato stojnost daj tezi produkti (projects)
        Project.findAll().then(projects=>{
            res.render("project/index", {"projects": projects})
        });


    },
    createGet: (req, res) => {
        res.render("project/create");
    },
    createPost: (req, res) => {
        let args = req.body.project;
        console.log(args);
        Project.create(args).then(()=>{
            res.redirect("/");
        })
    },
    editGet: (req, res) => {
        let id = req.params.id;

        Project.findById(id).then(project => {
            res.render("project/edit", {"project":project});
        })
    },

    editPost: (req, res) => {
        let id = req.params.id;
        let args = req.body.project;

        Project.findById(id).then(project=>{
            project.updateAttributes(args).then(()=>{
                res.redirect('/');
            })
        })
    },

    deleteGet: (req, res) => {
        let id = req.params.id;

        Project.findById(id).then(project => {
            res.render("project/delete", {"project":project});
        })
    },
    deletePost: (req, res) => {
        let id = req.params.id;

        Project.findById(id).then(project=>{
            project.destroy().then(()=>{
                res.redirect('/');
            })
        })
    }
};

------


Shopping List


----------

const Product = require('../models').Product;

module.exports = {
	index: (req, res) => {
		Product.findAll().then(entries => {
            res.render("product/index", {"entries": entries})
		})
	},

	createGet: (req, res) => {
		res.render("product/create");
	},

	createPost: (req, res) => {
		let body = req.body;

		Product.create(body).then(() => {
			res.redirect("/");
		})
	},

	editGet: (req, res) => {
		let id = req.params.id;

		Product.findById(id).then(product => {
            res.render("product/edit", product.dataValues);
		})

	},

	editPost: (req, res) => {
        let id = req.params.id;
        let body = req.body;

        Product.findById(id).then(product => {
			product.updateAttributes(body).then(() => {
				res.redirect("/");
			})
		})
	},

    deleteGet: (req,res) => {
        let id = req.params.id;

        Product.findById(id).then(product => {
            res.render("product/delete", product.dataValues);
        })
	},

    deletePost: (req,res) => {
        let id = req.params.id;

        Product.findById(id).then(product => {
        	product.destroy().then(()=> {
        		res.redirect("/");
			})
		})

    },

};

-----------------



   *********

Допълнителна помощ:

https://expressjs.com/en/guide/routing.html

Node.js is a platform written in JavaScript and provides back-end functionality. Express is a module (for now we can associate module as a class which provides some functionality) which wraps Node.js in way that makes coding faster and easier and it is suitable for MVC architecture.
Node.js | Express.js | Sequelize | Handlebars.js | MySQL | IDE: WEBStorm

***

app.METHOD(PATH, HANDLER)

// GET method route
app.get('/', function (req, res) {
  res.send('GET request to the homepage')
})

или съкратено:

app.get('/', (req, res) => {
  res.send('Hello World!')
})
// POST method route
app.post('/', function (req, res) {
  res.send('POST request to the homepage')
})


***

1. Restore dependencies:
a) cmd в папката на проекта
b) npm install

2. Shift Shift TODO - Todo-tata се проверяват

3.Model Definition
http://docs.sequelizejs.com/manual/tutorial/models-definition.html

4. Базата в Heidi - името е от config/config.json или config/config.js

5. ECMA 6 - ако свети models/index.js.: Unfortunately, when we use "let" it gets highlighted in red. This is because we have to switch our JavaScript version to ECMAScript 6. Press [Alt + Enter] to popup the helper, then click [Enter] and everything should be fine.
Now create a file models/user.js. Here we are going to define our User:


























*** More Entities:

------

const Sequelize = require('sequelize');
const encryption = require("../utilities/encryption");

module.exports = function (sequelize) {
    const User = sequelize.define('User', {
        email: {
            type: Sequelize.STRING,
            required: true,
            unique: true,
            allowNull: false
        },
        passwordHash: {
            type: Sequelize.STRING,
            required: true
        },
        fullName: {
            type: Sequelize.STRING,
            required: true
        },
        salt: {
            type: Sequelize.STRING,
            required: true
        },

    }, {
        timestamps: false
    });

    User.prototype.authenticate = function (password) {
        let inputPasswordHash = encryption.hashPassword(password, this.salt);
        return inputPasswordHash === this.passwordHash;
    };

    User.associate = function (models) {
        User.hasMany(models.Article, {
            foreignKey: 'authorId',
            sourceKey: 'id'
        })
    };


    return User;


};

------

const Sequelize = require('sequelize');

module.exports = function (sequelize) {
    const Article = sequelize.define('Article', {
        title: {
            type: Sequelize.STRING,
            required: true,
            allowNull: false
        },
        content: {
            type: Sequelize.TEXT,
            required: true,
            allowNull: false
        },
        date: {
            type: Sequelize.DATE,
            required: true,
            allowNull: false,
            defaultValue: Sequelize.NOW
        }

    }, {
        timestamps: false
    });

    Article.associate = function (models) {
        Article.belongsTo(models.User, {
            foreignKey: 'authorId',
            targetKey: 'id'
        })
    };

    return Article;
    
};

------



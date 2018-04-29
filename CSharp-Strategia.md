#### C#
Entity Framework | ASP.NET MVC | Razor | IDE: Visual Studio
launch.settings - port смяна
dbcontext - базата - но не се създава от нас

Ctrl + F5 - прекомпилира проекта
TODO - Меню: View / Task List

   *********

1. Пускаме *.sln файла

   *********

2. TODO: View / Task List

   *********

3. Entity (Model)

Тук имената са с главна | типовете с малка | има { get; set; }  | всичко е public (в JAVA e private)

[Key]
public int Id { get; set };

public double Price { get; set; }




За класовете в Entity:
pror tab tab
[Key]
public int Id { get; set; }


--------------------------

Entity Rider:

--------------------------


namespace ProjectRider.Models
{
    using System.ComponentModel.DataAnnotations;

    public class Project
    {
        [Key]
        public int Id { get; set; }

        public string Title { get; set; }

        public string Description { get; set; }

        public int Budget { get; set; }
    }
}


--------------------------

Entity Shopping List:

--------------------------

namespace ShoppingList.Models
{
    using System.ComponentModel.DataAnnotations;

    public class Product
    {
        [Key]
        public int Id { get; set; }

        public int Priority { get; set; }

        [Required]
        public string Name { get; set; }

        public int Quantity { get; set; }

        [Required]
        public string Status { get; set; }

    }
}


-------------------------


   *********

4. Controller

Da znaem:
View() тук връща: Project / Index (ot ProjectController i Index kato action)

        public ProjectController(ProjectDbContext context)
        {
            this.context = context;
        }

        [HttpGet]
        [Route("")]
        public ActionResult Index()
        {
            return View();
        }

------------------------------

Controller Rider:

------------------------------

namespace ProjectRider.Controllers
{
    using Microsoft.AspNetCore.Mvc;
    using ProjectRider.Models;
    using System.Linq;

    public class ProjectController : Controller
    {
        private readonly ProjectDbContext context;

        public ProjectController(ProjectDbContext context)
        {
            this.context = context;
        }

        [HttpGet]
        [Route("")]
        public ActionResult Index()
        {
            var projects = context.Projects.ToList();
            return View(projects);
        }



        [HttpGet]
        [Route("create")]
        public ActionResult Create()
        {
            return View();
        }



        [HttpPost]
        [Route("create")]
        public ActionResult Create(Project project)
        {
            context.Projects.Add(project);
            context.SaveChanges();
            return RedirectToAction("Index");
        }



        [HttpGet]
        [Route("edit/{id}")]
        public ActionResult Edit(int id)
        {
            Project project = context
                .Projects
                .Where(p => p.Id == id)
                .FirstOrDefault();

            return View(project);
        }



        [HttpPost]
        [Route("edit/{id}")]
        [ValidateAntiForgeryToken]
        public ActionResult EditConfirm(int id, Project projectModel)
        {
            context.Projects.Update(projectModel);
            context.SaveChanges();
            return RedirectToAction(nameof(Index));
        }



        [HttpGet]
        [Route("delete/{id}")]
        public ActionResult Delete(int id)
        {
            Project project = context
                .Projects
                .Where(p => p.Id == id)
                .FirstOrDefault();

            return View(project);
        }



        [HttpPost]
        [Route("delete/{id}")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirm(int id, Project projectModel)
        {
            context.Projects.Remove(projectModel);
            context.SaveChanges();
            return RedirectToAction("Index");
        }
    }
}




------------------------------

Controller Shopping List:

------------------------------

namespace ShoppingList.Controllers
{
    using Microsoft.AspNetCore.Mvc;
    using System.Collections.Generic;
    using System.Linq;
    using Models;

    public class ProductController : Controller
    {
        private readonly ShoppingListDbContext db;

        public ProductController(ShoppingListDbContext dbContext)
        {
            this.db = dbContext;
        }

        [HttpGet]
        [Route("")]
        public IActionResult Index()
        {
            var products = this.db.Products.ToList();

            return View(products);
        }

        [HttpGet]
        [Route("/create")]
        public IActionResult Create()
        {
            return View();
        }

        [HttpPost]
        [Route("/create")]
        public IActionResult Create(Product product)
        {

            if (ModelState.IsValid)
            {
                this.db.Products.Add(product);
                this.db.SaveChanges();
                return Redirect("/");
            }

            return View(product);
        }

        [HttpGet]
        [Route("/edit/{id}")]
        public IActionResult Edit(int? id)
        {
            var project = this.db.Products.Find(id);

            if(project == null)
            {
                return NotFound(404);
            }
            
            return View(project);
        }

        [HttpPost]
        [Route("/edit/{id}")]
        public IActionResult Edit(Product product)
        {
                this.db.Products.Update(product);
                this.db.SaveChanges();
                return Redirect("/");
         
        }

        [HttpGet]
        [Route("/delete/{id}")]
        public IActionResult Delete(int? id)
        {
            var project = this.db.Products.Find(id);

            if (project == null)
            {
                return NotFound(404);
            }

            return View(project);
        }

        [HttpPost]
        [Route("/Delete/{id}")]
        public IActionResult Delete(Product product)
        {
            this.db.Products.Remove(product);
            this.db.SaveChanges();
            return Redirect("/");

        }

    }
}





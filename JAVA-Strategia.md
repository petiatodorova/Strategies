#### JAVA
Spring MVC | Hybernate | Thymeleaf | MySQL | IDE: Intellij IDEA

Базата е в application.propertis da ima devtools
v html view-tata сменяма na ECMA6 (ако има нужда)

Ctrl + F9 - прекомпилира проекта
малки букви | не правим базата в HEIDI | слагаме си и id

   ********* 

1. Intellij - Import!  

   ********* 
   
2. TODO: Shift Shift TODO

   ********* 

3. Entity: 

@Column(name = "GRAND_TOTAL_AMOUNT", precision=8, scale=2)
private Double grandTotalAmount;

    !!!Задължително!!! За id:
--- @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;
    
    другите:
    @Column(nullable=false) ако се иска по условие или само:
    @Column

--- private Double price

--- private String name

--- private int neshtosi - ползваме ако е notnull - samo числени стойности
--- private Integer neshtosi - samo числени стойности - може и празно

    
    За останалите:
--- @Column

--- Alt + Ins - getters i setters na vsichko

--- Alt + Ins - Constructor na vsichko

--- Alt + Ins - Tab None - празен Constructor


---------------------------------------

Entity Rider:

---------------------------------------

package projectrider.entity;

import javax.persistence.*;

@Entity
@Table(name = "projects")
public class Project {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    @Column(nullable=false)
    private String title;

    @Column
    private String description;

    @Column
    private Integer budget;

    public Project() {
    }

    public Project(String title, String description, Integer budget) {
        this.title = title;
        this.description = description;
        this.budget = budget;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public Integer getBudget() {
        return budget;
    }

    public void setBudget(Integer budget) {
        this.budget = budget;
    }
}


---------------------------------------

Entity Shopping List:

---------------------------------------

package shoppinglist.entity;

import javax.persistence.*;

@Entity
@Table(name = "products")
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    private int priority;

    @Column(nullable = false)
    private String name;

    private int quantity;

    @Column(nullable = false)
    private String status;

    public Product() {
    }

    public Product(int priority, String name, int quantity, String status) {
        this.priority = priority;
        this.name = name;
        this.quantity = quantity;
        this.status = status;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public int getPriority() {
        return priority;
    }

    public void setPriority(int priority) {
        this.priority = priority;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getQuantity() {
        return quantity;
    }

    public void setQuantity(int quantity) {
        this.quantity = quantity;
    }

    public String getStatus() {
        return status;
    }

    public void setStatus(String status) {
        this.status = status;
    }
}


---------------------------------------


   ********* 


4. Binding modela: **********  

--- Copy ot Entity na poletata (bez id)

--- Cancel (za importa)

--- Delete (na @...)

--- Alt + Ins - getters i setters na vsichko

--- Alt + Ins - Constructor na vsichko

--- Alt + Ins - Tab None - празен Constructor



---------------------------------------

Binding Model Rider:

---------------------------------------

package projectrider.bindingModel;

public class ProjectBindingModel {

    private String title;

    private String description;

    private Integer budget;

    public ProjectBindingModel() {
    }

    public ProjectBindingModel(String title, String description, Integer budget) {
        this.title = title;
        this.description = description;
        this.budget = budget;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public Integer getBudget() {
        return budget;
    }

    public void setBudget(Integer budget) {
        this.budget = budget;
    }
}


---------------------------------------

Binding Model Shopping List:

---------------------------------------

package shoppinglist.bindingModel;

import javax.validation.constraints.NotNull;

public class ProductBindingModel {

    private int priority;

    @NotNull
    private String name;

    private int quantity;

    @NotNull
    private String status;

    public int getPriority() {
        return this.priority;
    }

    public void setPriority(int priority) {
        this.priority = priority;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getQuantity() {
        return quantity;
    }

    public void setQuantity(int quantity) {
        this.quantity = quantity;
    }

    public String getStatus() {
        return status;
    }

    public void setStatus(String status) {
        this.status = status;
    }
}

---------------------------------------

   ********* 


5. Controller: 

View-tata sa v resources / templates / project

---------------------------------------

Controller Rider:

---------------------------------------


package projectrider.controller;

import projectrider.entity.Project;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import projectrider.bindingModel.ProjectBindingModel;
import projectrider.repository.ProjectRepository;

import java.util.List;

@Controller
public class ProjectController {
    private final ProjectRepository projectRepository;

    @Autowired
    public ProjectController(ProjectRepository projectRepository) {
        this.projectRepository = projectRepository;
    }


    @GetMapping("/")
    public String index(Model model) {
        List<Project> projects = projectRepository.findAll();
        //"projects" идва от view-to като име
        model.addAttribute("projects", projects);
        model.addAttribute("view", "project/index");

        return "base-layout";
    }


    @GetMapping("/create")
    public String create(Model model) {
        model.addAttribute("view", "project/create");
        return "base-layout";
    }


//сетваме каквото ни трябва през ProjectBindingModel projectBindingModel
    @PostMapping("/create")
    public String createProcess(Model model, ProjectBindingModel projectBindingModel) {

        Project project = new Project();

        project.setTitle(projectBindingModel.getTitle());
        project.setDescription(projectBindingModel.getDescription());
        project.setBudget(projectBindingModel.getBudget());

        projectRepository.saveAndFlush(project);

        return "redirect:/";
    }


    @GetMapping("/edit/{id}")
    public String edit(Model model, @PathVariable int id) {

        Project project = projectRepository.findOne(id);
        model.addAttribute("project", project);
        model.addAttribute("view", "project/edit");
        return "base-layout";
    }


    @PostMapping("/edit/{id}")
    public String editProcess(@PathVariable int id, Model model, ProjectBindingModel projectBindingModel) {

        Project project = projectRepository.findOne(id);
        project.setTitle(projectBindingModel.getTitle());
        project.setDescription(projectBindingModel.getDescription());
        project.setBudget(projectBindingModel.getBudget());

        projectRepository.saveAndFlush(project);

        return "redirect:/";
    }


    @GetMapping("/delete/{id}")
    public String delete(Model model, @PathVariable int id) {

        Project project = projectRepository.findOne(id);
        model.addAttribute("project", project);
        model.addAttribute("view", "project/delete");
        return "base-layout";
    }


    @PostMapping("/delete/{id}")
    public String deleteProcess(@PathVariable int id, ProjectBindingModel projectBindingModel) {
        projectRepository.delete(id);

        return "redirect:/";
    }
}


---------------------------------------

Controller Shopping List:

---------------------------------------

package shoppinglist.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import shoppinglist.bindingModel.ProductBindingModel;
import shoppinglist.entity.Product;
import shoppinglist.repository.ProductRepository;

import java.util.List;

@Controller
public class ProductController {

    private final ProductRepository productRepository;

    @Autowired
    public ProductController(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    @GetMapping("/")
    public String index(Model model) {
        List<Product> productList =
                this.productRepository.findAll();

        model.addAttribute("view", "product/index");
        model.addAttribute("products", productList);

        return "base-layout";
    }

    @GetMapping("/create")
    public String create(Model model) {
        model.addAttribute("view", "product/create");
        return "base-layout";
    }

    @PostMapping("/create")
    public String createProcess(Model model, ProductBindingModel productBindingModel) {

        Product product = new Product(
                productBindingModel.getPriority(),
                productBindingModel.getName(),
                productBindingModel.getQuantity(),
                productBindingModel.getStatus()
        );

        this.productRepository.saveAndFlush(product);
        return "redirect:/";
    }

    @GetMapping("/edit/{id}")
    public String edit(Model model, @PathVariable int id) {

        if(!this.productRepository.exists(id)){
            return "redirect:/";
        }

        Product product = this.productRepository.findOne(id);

        model.addAttribute("view", "product/edit");
        model.addAttribute("product", product);
        return "base-layout";
    }

    @PostMapping("/edit/{id}")
    public String editProcess(Model model, @PathVariable int id, ProductBindingModel productBindingModel) {
        if(!this.productRepository.exists(id)){
            return "redirect:/";
        }

        Product product = this.productRepository.findOne(id);

        product.setPriority(productBindingModel.getPriority());
        product.setName(productBindingModel.getName());
        product.setQuantity(productBindingModel.getQuantity());
        product.setStatus(productBindingModel.getStatus());

        this.productRepository.saveAndFlush(product);
        return "redirect:/";
    }

    @GetMapping("/delete/{id}")
    public String delete(Model model, @PathVariable int id) {

        if(!this.productRepository.exists(id)){
            return "redirect:/";
        }

        Product product = this.productRepository.findOne(id);

        model.addAttribute("view", "product/delete");
        model.addAttribute("product", product);
        return "base-layout";
    }

    @PostMapping("/delete/{id}")
    public String deleteProcess(@PathVariable int id) {
        if(!this.productRepository.exists(id)){
            return "redirect:/";
        }

        Product product = this.productRepository.findOne(id);
        this.productRepository.delete(product);
        return "redirect:/";
    }


}


---------------------------------------

   ********* 

6. ProjectRiderApplication - десен бутон RUN  **********  

---------------------
   

Базата е в application.propertis da ima devtools
v html view-tata сменяма na ECMA6 (ако има нужда)

Model model - за да се подават на view-то разни неща, те се set-ват в model от тип Model и после чрез Controllers се рендерират.
model.addAttribute("username", "Ivan") (дава на променливата $username стойност Ivan или стойността на друга променлива и после рендерира).

@RequestMapping("/home") map-ва URL.


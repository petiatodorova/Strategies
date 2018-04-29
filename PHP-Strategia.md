#### PHP : PHPStorm - composer install

База - ние!
Id - avtomat!



TODO - има си бутон долу
config/parameters - показва коя е базата

Symfony | Doctrine | Twig | MySQL | IDE: PHPStorm

   ************************

1. PHPStorm - Open - Enable Symfony непременно!

   ************************

2. composer install
   ECMA Script 6 - ако подчертава нещо в някой JS script във view-tata
   ************************

3. Базата се създава в HEIDI - от app/config/parameters.yml (може и накрая от console)

   Изкарва всички команди за doctrine:
   
   php bin/console doctrine

   За базата: първо create и posle update:
   
   php bin/console doctrine:schema:create --if-not-exists
   php bin/console doctrine:schema:update --force 

   php bin/console server:run
   
   ************************

4. TODO: Shift Shift TODO

   ************************

5. Entity:

   Не създаваме id!

   src\AppBundle\Entity
   Десен бутон - DELETE - за да го изтрием и направим отново.
   
   Създава entity:
   
   php bin/console doctrine:generate:entity 
   
   после: SoftUniBlogBundle:Article
   
   imeNaApp:entity
   AppBundle:Project -> главна за entity името и малки за отделните свойства
   
   
   float e double
   
   /**
    * @var float
    *
    */
   
   DataTypes in Doctrine:
   https://www.doctrine-project.org/projects/doctrine-dbal/en/2.7/reference/types.html
   
   -----------
   
   Entity Rider:
   
   -----------

<?php

namespace AppBundle\Entity;

use Doctrine\ORM\Mapping as ORM;

/**
 * Project
 *
 * @ORM\Table(name="project")
 * @ORM\Entity(repositoryClass="AppBundle\Repository\ProjectRepository")
 */
class Project
{
    /**
     * @var int
     *
     * @ORM\Column(name="id", type="integer")
     * @ORM\Id
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    private $id;

    /**
     * @var string
     *
     * @ORM\Column(name="title", type="text")
     */
    private $title;

    /**
     * @var string
     *
     * @ORM\Column(name="description", type="text")
     */
    private $description;

    /**
     * @var int
     *
     * @ORM\Column(name="budget", type="integer")
     */
    private $budget;


    /**
     * Get id.
     *
     * @return int
     */
    public function getId()
    {
        return $this->id;
    }

    /**
     * Set title.
     *
     * @param string $title
     *
     * @return Project
     */
    public function setTitle($title)
    {
        $this->title = $title;

        return $this;
    }

    /**
     * Get title.
     *
     * @return string
     */
    public function getTitle()
    {
        return $this->title;
    }

    /**
     * Set description.
     *
     * @param string $description
     *
     * @return Project
     */
    public function setDescription($description)
    {
        $this->description = $description;

        return $this;
    }

    /**
     * Get description.
     *
     * @return string
     */
    public function getDescription()
    {
        return $this->description;
    }

    /**
     * Set budget.
     *
     * @param int $budget
     *
     * @return Project
     */
    public function setBudget($budget)
    {
        $this->budget = $budget;

        return $this;
    }

    /**
     * Get budget.
     *
     * @return int
     */
    public function getBudget()
    {
        return $this->budget;
    }
}


************************
   
6. Controller - преписваме, като си гледаме какво се вика горе в контролера и какво има във view-tata

Controller:
винаги html.twig: "project/index.html.twig"
app/Resources/views - view-tata
ProjectType - името на формата
$project или $projects според view-то

---------

Controller Rider:

------

<?php
namespace AppBundle\Controller;
use AppBundle\Entity\Project;
use AppBundle\Form\ProjectType;
use AppBundle\Repository\ProjectRepository;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;
use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\Request;
class ProjectController extends Controller
{
    /**
     * @Route("/", name="homepage")
     */
    public function index(Request $request)
    {
    
    //гледаме си във view-to какво търси (в случая $projects)
    //{% for project in projects %}
    //задаваме си го - $projects 
    // от този ->getDoctrine()->getRepository(//нашия клас//Project::class)->findAll();
    //return $this->render("project/index.html.twig", ['projects'=>$projects]);
        $projects = $this
            ->getDoctrine()
            ->getRepository(Project::class)
            ->findAll();
        return $this->render("project/index.html.twig", ['projects'=>$projects]);
    }
    
    
    
    /**
     * @Route("/create", name="create")
     */
    public function create(Request $request)
    {
    //тук си създаваме нов $project и после го зареждаме във формата $form = $this->createForm(ProjectType::class,$project); т.н.
    //$form->handleRequest($request); е с подадената променлива (Request $request)
    //дали е така Valid? 
    //$em е нашия entity manager
    
        $project = new Project();
        $form = $this->createForm(ProjectType::class,$project);
        $form->handleRequest($request);
        if($form->isSubmitted() §§ $form->isValid()){
            $em = $this->getDoctrine()->getManager();
            $em->persist($project);
            $em->flush();
           return $this->redirect('/');
        }
        return $this->render("project/create.html.twig",["form"=>$form->createView()]);
    }
    
    
    
    /**
     * @Route("/edit/{id}", name="edit")
     */
    public function edit($id, Request $request)
    {
        $project = $this
            ->getDoctrine()
            ->getRepository(Project::class)
            ->find($id);
        $form = $this->createForm(ProjectType::class,$project);
        $form->handleRequest($request);
        if($form->isSubmitted()){
            $em = $this->getDoctrine()->getManager();
            
            //moje i persist($project)
            $em->merge($project);
            $em->flush();
            return $this->redirect('/');
        }
        return $this->render('project/edit.html.twig',["form"=>$form->createView(), 'project'=>$project]);
    }
    
    
    
    /**
     * @Route("/delete/{id}", name="delete")
     * @param $id
     * @param Request $request
     * @return \Symfony\Component\HttpFoundation\RedirectResponse|\Symfony\Component\HttpFoundation\Response
     */
    public function delete($id, Request $request)
    {
    
    //copy ot edit - smiana na remove($project) i delete.html.twig
    //$project е от view-to
        $project = $this->getDoctrine()->getRepository(Project::class)->find($id);
        $form = $this->createForm(ProjectType::class,$project);
        $form->handleRequest($request);
        if($form->isSubmitted()){
            $em = $this->getDoctrine()->getManager();
            $em->remove($project);
            $em->flush();
            return $this->redirect('/');
        }
        return $this->render("project/delete.html.twig",["form"=>$form->createView(), 'project'=>$project]);
    }
}


------------

Controller Shopping List:

------------

<?php

namespace AppBundle\Controller;

use AppBundle\Entity\Product;
use AppBundle\Form\ProductType;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;
use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\Request;

class ProductController extends Controller
{
    /**
     * @param Request $request
     * @Route("/", name="index")
     * @return \Symfony\Component\HttpFoundation\Response
     */
    public function index(Request $request)
    {
		$products = $this->getDoctrine()->getRepository(Product::class)->findAll();

		return $this->render("product/index.html.twig",
           ['products' => $products] );
    }

    /**
     * @param Request $request
     * @Route("/create", name="create")
     * @return \Symfony\Component\HttpFoundation\Response
     */
    public function create(Request $request)
    {
		$product = new Product();

		$form = $this->createForm(ProductType::class, $product);
		$form->handleRequest($request);

		if($form->isSubmitted() && $form->isValid()){
		    $em = $this->getDoctrine()->getManager();
		    $em->persist($product);
		    $em->flush();
            return $this->redirect("/");
        }

        return $this->render("product/create.html.twig",
             ['form' => $form->createView()]);
    }

    /**
     * @Route("/edit/{id}", name="edit")
     *
     * @param $id
     * @param Request $request
     * @return \Symfony\Component\HttpFoundation\Response
     */
    public function edit($id, Request $request)
    {
        $product = $this->getDoctrine()->getRepository(Product::class)->find($id);

        $form = $this->createForm(ProductType::class, $product);
        $form->handleRequest($request);

        if($form->isSubmitted() && $form->isValid()){

            $em = $this->getDoctrine()->getManager();
            $em->persist($product);
            $em->flush();

            return $this->redirect("/");
        }

        return $this->render("product/delete.html.twig",
            ['form' => $form->createView(),
                'product' => $product]);
    }

    /**
     * @Route("/delete/{id}", name="delete")
     *
     * @param $id
     * @param Request $request
     * @return \Symfony\Component\HttpFoundation\Response
     */
    public function delete($id, Request $request)
    {
        $product = $this->getDoctrine()->getRepository(Product::class)->find($id);

        $form = $this->createForm(ProductType::class, $product);
        $form->handleRequest($request);

        if($form->isSubmitted() && $form->isValid()){

            $em = $this->getDoctrine()->getManager();
            $em->remove($product);
            $em->flush();

            return $this->redirect("/");
        }

        return $this->render("product/delete.html.twig",
            ['form' => $form->createView(),
                'product' => $product]);

    }
}



************************
    
   
7. Форма

!!! Symfony using а не doctrine ala-bala

   Първо delete.

   Създава формата: AppBundle:Film
   php bin/console doctrine:generate:form AppBundle:Film
   
   !!! Delete na poslednoto - prefix

   get block prefix - trie se vyv formata
   
   This code takes the data from request (make sure the imported “use” statement at the beginning of the class is Symfony\Component\HttpFoundation\Request) and fills the form.
   
   ************************
   
8. За базата: първо create и posle update:

   След създаване на Entity или преди старт - oбновяваме базата, за да ни влязат колонките:
   php bin/console doctrine:schema:update --force
   
   php bin/console doctrine:schema:create --if-not-exists
   php bin/console doctrine:schema:update --force 

   Старт:
   php bin/console server:run
   
   или:
   Edit
   PHPScript
   File: път до bin\console
   Arguments: server:run
   

-----------------------------------------------------

След преименуване на това-онова с shift-f6:
composer dump-autoload

----------------------------------------------------




-----------------------------------------------------

Създава базата данни
php bin/console doctrine:schema:update --force

set PATH=%PATH%;C:\xampp\php\php.exe


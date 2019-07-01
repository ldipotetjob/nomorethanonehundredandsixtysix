---
title: "Pattern Matching"
date: 2019-07-01T11:36:33+08:00
draft: false
featuredImg: ""
tags: 
  - Pattern Matching 
  - spec 2.12
---


Some examples of patterns are:[^1]

1. The pattern ex: IOException matches all instances of class  IOException, binding variable ex to the instance.
2. The pattern Some(x) matches values of the form Some(v), binding x to the argument value v of the Some constructor.
3. The pattern (x, _) matches pairs of values, binding x to the first component of the pair. The second component is matched with a wildcard pattern.
4. The pattern x :: y :: xs matches lists of length ≥2, binding x to the list's first element, y to the list's second element, and xs to the remainder.
5. The pattern 1 | 2 | 3 matches the integers between 1 and 3.

---
Variable Patterns:

>SimplePattern   ::=  ‘_’  
   |  varid [^2]

```javascript
import scala.util.Random
val x: Int = Random.nextInt(10)

x match {
  case 0 => "zero"
  case 1 => "one"
  case 2 => "two"
  case _ => "many"
}
// the result can be any value is a random function

def matchTest(x: Int): String = x match {
  case 1 => "one"
  case 2 => "two"
  case _ => "many"
}
matchTest(3) //res1: String = many
matchTest(1) //res2: String = one
```

Typed Patterns:

>Pattern1        ::=  varid ‘:’ TypePat  
                    |  ‘_’ ‘:’ TypePat [^3]

```javascript
val mystrType: String = "String value for Pattern matching"

mystrType match {
  case tmpstrValue:String=>s"String value: $tmpstrValue"
  case _ => "Any other str value"
}

val testListType:List[Int] = List(1,2,3,4)

testListType match {
  case tmpListIntValue:List[Int]=> s"List[Int] value: $tmpListIntValue"
  case _ => "No list[Int] Type"
}

testListType match {
  case tmpListgenericValue:List[_]=> s"List[_] generic value: $tmpListgenericValue"
  case _ => "No list[Int] Type"
}

// mystrType: String = String value for Pattern matching
// res3: String = String value: String value for Pattern matching

```

Pattern Binders:

>Pattern2        ::=  varid ‘@’ Pattern3 [^4]

```javascript
/**
  *   
  *   is very useful is the var tha you want to match can have different type of values and
  *   at the same time you need to discriminate de type and process its values at the same time
  *   A pattern binder x@p consists of a pattern variable x and a pattern p.
  *   The type of the variable x is the static type T of the pattern p.
  *   Is very important to KNOW that this pattern matches any value v matched by the pattern p, provided the
  *   run-time type of v is also an instance of T, and it binds the variable name to that value.
  *
  */

val testListBinding:List[Int] = List(1,2,3,4)

testListBinding match {
  case testingList@List(1, _*) => s"list start with1, we're testing binging with this list:$testingList"
  case testingList@List(3, _*) => s"list start with3, we're testing binging with this list:$testingList"
  case _ => " nothing match  binding"
}

// testListBinding: List[Int] = List(1, 2, 3, 4)
// res3: String = list start with1, we're testing binging with this list:List(1, 2, 3, 4)

val optBindingStrval:String = "Testing scala specification"
val optbinding: Option[String] = Some(optBindingStrval)

optbinding match {
  case optbinding@Some(optStrValue) => optbinding.getOrElse("wrong value")
  case _ =>"no optStrValue"
}

```

Literal Patterns:

>SimplePattern   ::=  StableId [^5]  

```javascript
val literalPattern: Any = true

literalPattern match {

  case 0 => "zero"
  case true => ""

}
```

Stable Identifier Patterns:

>SimplePattern   ::=  StableId [^6]

```javascript
def f(x: Int, y: Int) = x match {

  /**
    * in this case y => s"$x and $y are equals"
    * the other cases are unreachable because always match with y
    */

  case `y` => s"$x and $y are equals"
  case  _  => "x and y are not equals"
}

f(5,5) // res9: String = 5 and 5 are equals

f(5,4) // res10: String = x and y are not equals

def mMatch(s: String,target: String) = {
  s match {
    case `target` => println("It was " + target)
    case _ => println("It was something else")
  }
}

mMatch("a","b")  // It was something else

def mMatchrefactored(s: String) = {
  val target: String = "a"
  val x = target
  s match {
    case `x` => println("It was " + target)
    case _ => println("It was something else")
  }
}

mMatchrefactored("a")

// It was a
// res12: Unit = ()

```

Constructor Patterns:

>SimplePattern   ::=  StableId ‘(’ [Patterns] ‘)’ [^7]

```javascript
/**
  * there are better implementations for these examples but the main idea is the explanation
  * of this pattern as easy as possible
  */

case class Student(name:String,age:Int,birtdate:String)

case class Teacher(name:String,age:Int,birtdate:String)

def studentOrTeacher(person: Any): Unit = {

  person match {
    case person@Student(name,age,birthdate) => println(s"the Student:$name is $age old and born $birthdate ")
    case person@Teacher(name,age,birthdate) => println(s"the Teacher:$name is $age old and born $birthdate ")
    case _  => println("Could be any one, No teacher or Person")
  }
}

studentOrTeacher(Student("Jose Luis",18,"28 feb 1971"))

//the Student:Jose Luis is 18 and born 28 feb 1971
// res13: Unit = ()


def studentOrTeacherVer1(person: Any): Unit = {

  person match {
    case Student(name,age,birthdate)=> println(s"the Student:$name is $age old and born $person.birthdate ")
    case Teacher(name,age,birthdate) => println(s"the Teacher:$name is $age old and born $birthdate ")
    case _  => println("Could be any one, No teacher or Person")
  }
}

studentOrTeacher(Teacher("Jose Luis",18,"28 feb 1971"))
Teacher.unapply(Teacher("Jose Luis",18,"28 feb 1971"))

//the Teacher:Jose Luis is 18 old and born 28 feb 1971
//res14: Unit = ()

```

Tuple Patterns:

>SimplePattern   ::=  ‘(’ [Patterns] ‘)’ [^8]

```javascript

/**
  * For this specification you'll need to know what is Tuple in scala.
  * If you don't know We recommend this link: https://docs.scala-lang.org/tour/tuples.html
  *
 */

val ingredient = ("Sugar" , 25)
val (name, quantity) = ingredient
val fruit = ("orange",10,"spain")
val (namefruit, amount,origin) = fruit

def anyObject(param:Any): Unit = {
  param  match {
    case (name,amount) => println(s"we are talking about the ingredients:$name")
    case (name,amount,origin) => println(s"we are talking about the fruits:$name")
  }
}

anyObject(fruit)
// we are talking about the fruits:orange
// res15: Unit = ()

anyObject(ingredient)

//we are talking about the ingredients:Sugar
//res16: Unit = ()

```

Extractor Patterns:

>  SimplePattern   ::=  StableId ‘(’ [Patterns] ‘)’ [^9]

```javascript
// For a better understanding of this pattern I recommend to read the following next link:
// https://docs.scala-lang.org/tour/extractor-objects.html

case class StudentVer(name:String,age:Int,birtdate:String)

// Internally what's going on? :

object StudentVer{
  def apply(name:String,age:Int,birtdate:String): StudentVer = new StudentVer(name,age,birtdate)
}

// when create en new case class
val myFirstStudentVer:StudentVer = StudentVer("Craig Page",20,"20 August,1999")

// Internally, it is calling the apply method of StudentVer1 companion object to create new StudentVer1 object:

val myFirstStudentVer1 = StudentVer.apply("Craig Page",20,"20 August,1999")

// The reverse operation to Apply could be the case in which we have the case class value and we want to
// know its values

val mytuplaOfUnapply = StudentVer.unapply(myFirstStudentVer1)

//val (namestudent,age,brithdate) = StudentVer.unapply(myFirstStudentVer1)

mytuplaOfUnapply match {

  case Some((namestudent,age,brithdate)) => println(s"Testing extractor name:$namestudent,age:$age,birthdate:$brithdate")
  case _ => println("nothing to show")

}

val StudentVer(nameStudentVer,ageStudentVer,birtdateStudentVer) = myFirstStudentVer1

println(s"Another example testing extractor name:$nameStudentVer,age:$ageStudentVer,birthdate:$ageStudentVer")

def studentOrTeacherUsingExtractors(person: Any): Unit = {

  //Pattern matching is using extractors in this point
  person match {
    case Student(name,age,birthdate) => println(s"the Student:$name is $age old and born $birthdate ")
    case Teacher(name,age,birthdate) => println(s"the Teacher:$name is $age old and born $birthdate ")
    case _  => println("Could be any one, No teacher or Person")
  }
}

```
  
  
  
  
  
            
[^1]: Pattern matching scala spec from https://www.scala-lang.org/files/archive/spec/2.12/08-pattern-matching.html
[^2]: Variable Patterns from https://www.scala-lang.org/files/archive/spec/2.12/08-pattern-matching.html#variable-patterns
[^3]: Typed Patterns from https://www.scala-lang.org/files/archive/spec/2.12/08-pattern-matching.html#typed-patterns
[^4]: Pattern Binders from https://www.scala-lang.org/files/archive/spec/2.12/08-pattern-matching.html#pattern-binders
[^5]: Literal Patterns from https://www.scala-lang.org/files/archive/spec/2.12/08-pattern-matching.html#literal-patterns
[^6]: Stable Identifier Patternsfrom https://www.scala-lang.org/files/archive/spec/2.12/08-pattern-matching.html#stable-identifier-patterns
[^7]: Constructor Patterns from https://www.scala-lang.org/files/archive/spec/2.12/08-pattern-matching.html#constructor-patterns
[^8]: Tuple Patterns from https://www.scala-lang.org/files/archive/spec/2.12/08-pattern-matching.html#tuple-patterns
[^9]: Extractor Patterns from https://www.scala-lang.org/files/archive/spec/2.12/08-pattern-matching.html#extractor-patterns 
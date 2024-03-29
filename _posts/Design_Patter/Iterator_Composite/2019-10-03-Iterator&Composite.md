---
title: 이터레이터 & 컴포짓 패턴
tags: ["디자인 패턴"]
author: pilsoo lee
markdown: kramdown
---

# 이터레이터

## 루와 멜의 메뉴 구현법
### 루의 코드 구현법
#### [코드 전체 보기](https://github.com/LostFinger/LostFinger.github.io/tree/master/_posts/Design_Patter/Iterator_Composite/DinerMenu.java)
> 루는 Array를 이용해 데이터를 보관
{% highlight java %}
public class PancakeHouseMenu implements Menu {
	ArrayList<MenuItem> menuItems;
    ~~~
	public ArrayList<MenuItem> getMenuItems() {
		return menuItems;
	}
}
{% endhighlight %}


### 멜의 메뉴 구현법
#### [코드 전체 보기](https://github.com/LostFinger/LostFinger.github.io/tree/master/_posts/Design_Patter/Iterator_Composite/DinerMenu.java)
> 멜은 배열을 사용
{% highlight java %}
public class DinerMenu implements Menu {
	static final int MAX_ITEMS = 6;
	int numberOfItems = 0;
	MenuItem[] menuItems;
	~~~~
	public MenuItem[] getMenuItems() {
		return menuItems;
	}
}

{% endhighlight %}

### 문제점
```
웨이터 입장 :
메뉴의 구현 방식이 다르며 루에게서 어레이를 받아 처리
멜에게서는 배열을 받아서 처리
루와 멜은 서로 코드를 바꾸기 싫어하는 상황.
```

## 이터레이터 패턴

```
 바뀌는(다른) 부분 : 배열과 리스트를 반환하는 부분
 => 변경되는 부분을 Iterator에 의존시켜 사용.
```
### Iterator Interface

{% highlight java %}
public interface Iterator {
	boolean hasNext();
	MenuItem next();
}
{% endhighlight %}


### Iterator class의 구상클래스 Diner Menu Iterator 생성
{% highlight java %}
public class DinerMenuIterator implements Iterator {
	MenuItem[] items;
	int position = 0;

	public DinerMenuIterator(MenuItem[] items) {
		this.items = items;
	}

	public MenuItem next() {
		MenuItem menuItem = items[position];
		position = position + 1;
		return menuItem;
	}

	public boolean hasNext() {
		if (position >= items.length || items[position] == null) {
			return false;
		} else {
			return true;
		}
	}
}
{% endhighlight %}
### DinerMenu 변경
![이미지](https://raw.githubusercontent.com/LostFinger/LostFinger.github.io/master/_posts/Design_Patter/Iterator_Composite/1.png)

### 웨이트리스 코드 수정
{% highlight java %}
public class Waitress {
    Menu pancakeHouseMenu;
    Menu dinerMenu;
    public Waitress(Menu pancakeHouseMenu, Menu dinerMenu) {
        this.pancakeHouseMenu = pancakeHouseMenu;
        this.dinerMenu = dinerMenu;
    }
    public void printMenu() {
        Iterator pancakeIterator = pancakeHouseMenu.createIterator();
        Iterator dinerIterator = dinerMenu.createIterator();
        System.out.println("MENU\n----\nBREAKFAST");
        printMenu(pancakeIterator);
        System.out.println("\nLUNCH");
        printMenu(dinerIterator);
    }
    private void printMenu(Iterator iterator) {
        while (iterator.hasNext()) {
            MenuItem menuItem = iterator.next();
            System.out.print(menuItem.getName() + ", ");
            System.out.print(menuItem.getPrice() + " -- ");
            System.out.println(menuItem.getDescription());
        }
    }
    ~~~
}
{% endhighlight %}

## 정리

### Iterator vs 기존 코드

| 기존 웨이트리스| 이터레이터 패턴의 웨이트리스|
|---|:---:|
| 메뉴가 캡슐화 되지 않아 각자의 데이터 형태를 알 수 있음 | 각 메뉴에서 어떤 컬렉션을 이용해서 구현했는지 모름|
| 각각의 반복작업을 위해 다른 두개의 구현체가 필요| Iterator 만 이용하고 있기 때문에 한개의 구현체로도 처리가 가능|
| 웨이트리스가 메뉴아이템에 직접 연결| 이터레이터 인터페이스만 알고 있다. |

### 지금까지 프로젝트 디자인

![이미지](https://raw.githubusercontent.com/LostFinger/LostFinger.github.io/master/_posts/Design_Patter/Iterator_Composite/2.png)


## 메뉴 인터페이스 통일
### 메뉴 인터페이스
{% highlight java %}
public interface Menu {
	public Iterator<MenuItem> createIterator();
}
{% endhighlight %}

### 정리
![이미지](https://raw.githubusercontent.com/LostFinger/LostFinger.github.io/master/_posts/Design_Patter/Iterator_Composite/3.png)


## 이터레이터 패턴의 정의

`컬렉션 구현 방법을 노출시키지 않고 그 집합체 안에 들어있는 모든 항목에 접근할 수 있는 방법을 제공`

### 구성도

![이미지](https://raw.githubusercontent.com/LostFinger/LostFinger.github.io/master/_posts/Design_Patter/Iterator_Composite/4.png)

## 단일 역할 원칙

### 디자인 원칙
`클래스를 바꾸는 이유는 한 가지 뿐이어야 한다.`

### 설명
``
원래 클래스의 역할(집합체 관리) 외에 다른 역할 (반복자 메소드) 를 처리하도록 하면
두가지 이유에 의해 클래스가 변경이 되야 하기 때문에
클래스를 나중에 고쳐야할 가능성이 증가한다.
``

### 응집도 (cohesion)
 한 클래스 또는 모듈이 특정 목적 또는 역할을 얼마나 일관되게 지원하는지 나타내는 척도.
  응집도가 높다라는 의미는 연관된 기능이 묶여있다라는 의미
  응집도가 낮다는 서로 상관없는 기능들이 묶여서 존재한다라는 것을 의미한다.




# 컴포짓 패턴

## 객체마을 카페 메뉴

### 새로운 메뉴의 추가

{% highlight java %}
public class CafeMenu {
    Hashtable menuItems = new HashTable();
    public CafeMenu() {
        ~~~
    }
    public Hashtable getItems(){
        return menuItems;
    }
}
{% endhighlight %}

> 이 카페 메뉴를 우리가 사용하는 프레임워크에 집어넣을 계획


### CafeMenu 코드 고치기
{% highlight java %}
public class CafeMenu implements Menu {
    Hashtable menuItems = new HashTable();
    public CafeMenu() {
        ~~~
    }
    public Hashtable getItems(){
        return menuItems;
    }
    =>
    public Iterator createIterator(){
        return menuItems.values().iterator();
    }
}
{% endhighlight %}

> 반복자로 변경해서 리턴.

### Waitress 에 카페 메뉴 추가하기
{% highlight java %}
public class Waitress {
    ~~~
    Menu CafeMenu;
    ~~

    public Waitress(Menu pancakeHouseMenu, Menu dinerMenu, Menu cafeMenu){
        this.pancakeHouseMenu = pancakeHouseMenu;
        this.dinerMenu = dinerMenu;
        this.cafeMenu = cafeMenu;
    }

    public void printMenu (){
        ~~~
        Iterator cafeIterator = cafeMenu.createIterator();
        System.out.println("\n 저녁 메뉴");
        printMenu(cafeIterator);
        ~~~
    }
}
{% endhighlight %}

### 내용 정리

#### 웨이트리스에서 각 객체의 자료형의 의존도를 끊음
#### iterator 를 사용해서 확장성을 강화함
#### Collection interface를 사용하는 것들을 이용할 수 있음.



### 좀더 수정

#### 각 객체가 추가될 때마다 printMenu 를 추가해야함

> OCP(Open Closed Principle)에 위배
여러 메뉴를 관리할 수 있는 방법이 필요
{% highlight java %}
public class Waitress {
    ~~~
    ArrayList menus;
    ~~

    public Waitress(ArrayList menus){
        this.menus = menus;
    }

    public void printMenu (){
        Iterator menuIterator = menus.iterator();
        while(menuIterator.hasNext()){
            Menu menu = (Menu)menuIterator.next();
            printMenu(menu.createIterator());
        }
    }
    ~~~
}
{% endhighlight %}

> 이로써 OCP 원칙에 위배되지 않고 객체의 추가가 가능

### But, 디저트 서브 메뉴를 추가해달라는 요청이 들어왔다!!

> 메뉴 안에 메뉴가 들어가 있는 것을 지원해 달라.

![이미지](https://raw.githubusercontent.com/LostFinger/LostFinger.github.io/master/_posts/Design_Patter/Iterator_Composite/5.png)

> 새로운 변경이 필요

### 필요한 추가 사항

#### 메뉴, 서브메뉴, 메뉴 항목 등을 모두 집어넣을 수 있는 트리 형태의 구조가 필요하다.
#### 각 메뉴에 있는 모든 항목에 대해서 돌아갈 수 있는 방법을 제공.
#### 아이템에 대해서 반복작업 수행이 가능


## 컴포짓 패턴의 정의

> 객체들을 트리 구조로 구성하여 부분과 전체를 나타내는 계층 구조로 만드는 것이 가능하다.
이 패턴을 이용하면 클라이언트에서 개별 객체와 다른 객체들로 구성된 복합 객체(coposite)를 똑같은 방법으로 다룰 수 있다.

![이미지](https://raw.githubusercontent.com/LostFinger/LostFinger.github.io/master/_posts/Design_Patter/Iterator_Composite/6.png)

### 다이어그램
![이미지](https://raw.githubusercontent.com/LostFinger/LostFinger.github.io/master/_posts/Design_Patter/Iterator_Composite/7.png)

## 컴포지트 패턴을 이용한 메뉴 디자인
![이미지](https://raw.githubusercontent.com/LostFinger/LostFinger.github.io/master/_posts/Design_Patter/Iterator_Composite/8.png)



## 메뉴 구성요소 구현

> MenuItem(잎) Menu(복합 객체)에서 각자 용도에 맞지 않아 구현할 필요가 없는 메소드에 대해서
그냥 기본 메소드를 사용할 수 있도록 기본 구현을 만들계획

### 추상 클래스

> 모든 메소드를 기본적으로 구현해 놓음
{% highlight java %}
public abstract class MenuComponent {
	public void add(MenuComponent menuComponent) {
		throw new UnsupportedOperationException();
	}
	public void remove(MenuComponent menuComponent) {
		throw new UnsupportedOperationException();
	}
	public MenuComponent getChild(int i) {
		throw new UnsupportedOperationException();
	}
	public String getName() {
		throw new UnsupportedOperationException();
	}
	public String getDescription() {
		throw new UnsupportedOperationException();
	}
	public double getPrice() {
		throw new UnsupportedOperationException();
	}
	public boolean isVegetarian() {
		throw new UnsupportedOperationException();
	}
	public void print() {
		throw new UnsupportedOperationException();
	}
}
{% endhighlight %}


### 메뉴 항목 구현
{% highlight java %}
public class MenuItem extends MenuComponent {
	String name;
	String description;
	boolean vegetarian;
	double price;
	public MenuItem(String name,
	                String description,
	                boolean vegetarian,
	                double price)
	{
		this.name = name;
		this.description = description;
		this.vegetarian = vegetarian;
		this.price = price;
	}
	public String getName() {
		return name;
	}
	public String getDescription() {
		return description;
	}
	public double getPrice() {
		return price;
	}
	public boolean isVegetarian() {
		return vegetarian;
	}
	public void print() {
		System.out.print("  " + getName());
		if (isVegetarian()) {
			System.out.print("(v)");
		}
		System.out.println(", " + getPrice());
		System.out.println("     -- " + getDescription());
	}
}
{% endhighlight %}

### 메뉴 구현

> getPrice() 와 같은 객체의 의미 없는 부분은 구현하지 않음.

{% highlight java %}
public class Menu extends MenuComponent {
	ArrayList<MenuComponent> menuComponents = new ArrayList<MenuComponent>();
	String name;
	String description;

	public Menu(String name, String description) {
		this.name = name;
		this.description = description;
	}

	public void add(MenuComponent menuComponent) {
		menuComponents.add(menuComponent);
	}

	public void remove(MenuComponent menuComponent) {
		menuComponents.remove(menuComponent);
	}

	public MenuComponent getChild(int i) {
		return (MenuComponent)menuComponents.get(i);
	}

	public String getName() {
		return name;
	}

	public String getDescription() {
		return description;
	}

	public void print() {
		System.out.print("\n" + getName());
		System.out.println(", " + getDescription());
		System.out.println("---------------------");

		Iterator<MenuComponent> iterator = menuComponents.iterator();
		while (iterator.hasNext()) {
			MenuComponent menuComponent =
				(MenuComponent)iterator.next();
			menuComponent.print();
		}
	}
}
{% endhighlight %}

> 반복작업 수행중 다른 메뉴가 나타날 경우 또다른 반복작업을 리커시브하게 수행한다.

### print() 메서드 고치기
{% highlight java %}
public void print(){
    Iterator iterator = menuComponents.iterator();
    while(iterator.hasNext()){
        menuComponent.print();
    }
}
{% endhighlight %}


### 메뉴 테스트
> 이 코드를 사용하게 될 클라이언트인 Waitress 코드를 수정
{% highlight java %}
public class Waitress {
    MenuComponent allMenus;
    public Waitress(MenuComponent allMenus) {
        this.allMenus = allMenus;
    }
    public void printMenu() {
        allMenus.print();
    }
}
{% endhighlight %}

![이미지](https://raw.githubusercontent.com/LostFinger/LostFinger.github.io/master/_posts/Design_Patter/Iterator_Composite/9.png)


### 테스트 코드
{% highlight java %}
public class MenuTestDrive {
	public static void main(String args[]) {
		MenuComponent pancakeHouseMenu =
			new Menu("PANCAKE HOUSE MENU", "Breakfast");
		MenuComponent dinerMenu =
			new Menu("DINER MENU", "Lunch");
		MenuComponent cafeMenu =
			new Menu("CAFE MENU", "Dinner");
		MenuComponent dessertMenu =
			new Menu("DESSERT MENU", "Dessert of course!");
		MenuComponent coffeeMenu = new Menu("COFFEE MENU", "Stuff to go with your afternoon coffee");
		MenuComponent allMenus = new Menu("ALL MENUS", "All menus combined");
		allMenus.add(pancakeHouseMenu);
		allMenus.add(dinerMenu);
		allMenus.add(cafeMenu);

		pancakeHouseMenu.add(new MenuItem(
			"K&B's Pancake Breakfast",
			"Pancakes with scrambled eggs, and toast",
			true,
			2.99));
		pancakeHouseMenu.add(new MenuItem(
			"Regular Pancake Breakfast",
			"Pancakes with fried eggs, sausage",
			false,
			2.99));
		pancakeHouseMenu.add(new MenuItem(
			"Blueberry Pancakes",
			"Pancakes made with fresh blueberries, and blueberry syrup",
			true,
			3.49));
		pancakeHouseMenu.add(new MenuItem(
			"Waffles",
			"Waffles, with your choice of blueberries or strawberries",
			true,
			3.59));
		dinerMenu.add(new MenuItem(
			"Vegetarian BLT",
			"(Fakin') Bacon with lettuce & tomato on whole wheat",
			true,
			2.99));
		dinerMenu.add(new MenuItem(
			"BLT",
			"Bacon with lettuce & tomato on whole wheat",
			false,
			2.99));
		dinerMenu.add(new MenuItem(
			"Soup of the day",
			"A bowl of the soup of the day, with a side of potato salad",
			false,
			3.29));
		dinerMenu.add(new MenuItem(
			"Hotdog",
			"A hot dog, with saurkraut, relish, onions, topped with cheese",
			false,
			3.05));
		dinerMenu.add(new MenuItem(
			"Steamed Veggies and Brown Rice",
			"Steamed vegetables over brown rice",
			true,
			3.99));
		dinerMenu.add(new MenuItem(
			"Pasta",
			"Spaghetti with Marinara Sauce, and a slice of sourdough bread",
			true,
			3.89));
		dinerMenu.add(dessertMenu);
		dessertMenu.add(new MenuItem(
			"Apple Pie",
			"Apple pie with a flakey crust, topped with vanilla icecream",
			true,
			1.59));
		dessertMenu.add(new MenuItem(
			"Cheesecake",
			"Creamy New York cheesecake, with a chocolate graham crust",
			true,
			1.99));
		dessertMenu.add(new MenuItem(
			"Sorbet",
			"A scoop of raspberry and a scoop of lime",
			true,
			1.89));
		cafeMenu.add(new MenuItem(
			"Veggie Burger and Air Fries",
			"Veggie burger on a whole wheat bun, lettuce, tomato, and fries",
			true,
			3.99));
		cafeMenu.add(new MenuItem(
			"Soup of the day",
			"A cup of the soup of the day, with a side salad",
			false,
			3.69));
		cafeMenu.add(new MenuItem(
			"Burrito",
			"A large burrito, with whole pinto beans, salsa, guacamole",
			true,
			4.29));
		cafeMenu.add(coffeeMenu);
		coffeeMenu.add(new MenuItem(
			"Coffee Cake",
			"Crumbly cake topped with cinnamon and walnuts",
			true,
			1.59));
		coffeeMenu.add(new MenuItem(
			"Bagel",
			"Flavors include sesame, poppyseed, cinnamon raisin, pumpkin",
			false,
			0.69));
		coffeeMenu.add(new MenuItem(
			"Biscotti",
			"Three almond or hazelnut biscotti cookies",
			true,
			0.89));
		Waitress waitress = new Waitress(allMenus);
		waitress.printMenu();
	}
}
{% endhighlight %}


### 설명

#### 컴포지트 패턴에서는 계층구조를 관리하는 일 & 메뉴 관련된 작업을 처리하는 일 둘 다 처리한다.

#### 단일 원칙을 깨고 대신 투명성을 확보.
> 투명성 : 어떤 원소가 복합객체인지 잎노드인지 클라이언트 입장에서는 투명하게 느껴진다.(똑같은 방식으로 처리)

## 이터레이터
### 모든 구성 요소에 createIterator() 메소드 추가.
![이미지](https://raw.githubusercontent.com/LostFinger/LostFinger.github.io/master/_posts/Design_Patter/Iterator_Composite/10.png)

### 실제 구현 객체
{% highlight java %}
public class Menu extends MenuComponent {
    // 나머지 코드는 그대로
   public Iterator createIterator(){
        return new CompositeIterator(menuComponents.iterator();
   }
}
{% endhighlight %}

### Null 객체
{% highlight java %}
public class Menu extends MenuComponent {
    // 나머지 코드는 그대로
   public Iterator createIterator(){
        return new NullIterator();
   }
}
{% endhighlight %}
## 복합 반복자
> CompositeIterator는 복합 객체 안에 들어있는 MenuItem에 대해 반복작업을 할 수 있게 해주는 기능을 제공.
{% highlight java %}
public class CompositeIterator implements Iterator<MenuComponent> {
	Stack<Iterator<MenuComponent>> stack = new Stack<Iterator<MenuComponent>>();

	public CompositeIterator(Iterator<MenuComponent> iterator) {
		stack.push(iterator);
	}

	public MenuComponent next() {
		if (hasNext()) {
			Iterator<MenuComponent> iterator = stack.peek();
			MenuComponent component = iterator.next();
			stack.push(component.createIterator());
			return component;
		} else {
			return null;
		}
	}

	public boolean hasNext() {
		if (stack.empty()) {
			return false;
		} else {
			Iterator<MenuComponent> iterator = stack.peek();
			if (!iterator.hasNext()) {
				stack.pop();
				return hasNext();
			} else {
				return true;
			}
		}
	}

	/*
	 * No longer needed as of Java 8
	 *
	 * (non-Javadoc)
	 * @see java.util.Iterator#remove()
	 *
	public void remove() {
		throw new UnsupportedOperationException();
	}
	*/
}
{% endhighlight %}



## 널 반복자
### Null 을 리턴?
> 클라이언트에서 Null 체크를 추가적으로 해야한다는 단점이 존재

### 널 반복자를 리턴
> hasNext() 가 호출될때 무조건 false 를 리턴하는 반복자를 리턴.
클라이언트에서는 Null 객체인지 신경 쓸 필요가 없다.


{% highlight java %}
public class NullIterator implements Iterator<MenuComponent> {
	public MenuComponent next() {
		return null;
	}
	public boolean hasNext() {
		return false;
	}
	public void remove() {
		throw new UnsupportedOperationException();
	}
}
{% endhighlight %}

### 채식 주의자 용 메뉴
{% highlight java %}
public void printVegetarianMenu(){
    Iterator iterator = allMenus.createIterator();

    while(iterator.hasNext()) {
        MenuComponent menuComponent = (MenuComponent) iterator.next();
        try {
            if( menuComponent.isVegetarian()) {
                menuComponent.print();
            }
        }catch(UnsupportedOperationException e) { }
    }

}

{% endhighlight %}

# 정리

## 이터레이터

### 장점

#### SRP, 클라이언트의 코드를 정리 할 수 있다 ( 하나의 함수로 관리할수 있다는 의미 )
#### OCP, 새로운 타입의 컬렉션 형태를 자유롭게 implementation 가능하다.
#### 각각 고유의 상태를 가지고 있기 때문에 병렬적인 Iterator 처리가 가능하다 ( 하지만 삭제나 추가에 주의가 필요할듯 )
#### 원한다면 같은이유로 이터레이터를 지연하거나 계속 할 수 있다.

### 단점

#### 어플리케이션이 간단한 컬렉션에서만 작동하는 경우 과도한 작업이 될 수 있다.
#### 몇몇의 특별한 콜렉션을 직접 사용하는것보다 이터레이터의 경우에 비효율적으로 동작할 가능성이 있다.

## 컴포짓 패턴

### 장점

#### 복잡한 트리형태의 구조에 대해 처리가 가능하며, 폴리모르피즘과 리커시브를 이용해서 장점을 극대화 할수 있다.
#### OCP 새로운 형태의 타입이 추가되더라도 문제가 없다.

### 단점

#### 기능이 너무 다른 경우에 추상화 클래스를 이용하기 까다롭다.
#### 특정 시나리오에서는 인터페이스를 과도하게 일반화 해야하기 때문에 코드를 이해하기 어려울 수 있다.


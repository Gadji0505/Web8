package djen;
import java.util.*;
/* Параметризированный массив. Методы-вывод элемента по его номеру, развернуть массив в обратном порядке, 
вывод части элементов массива от начала до заданного индекса*/

class Mass<T>{
T[]a;
Mass(T []a)
{
this.a=a.clone();
}
void print ()
{
    System.out.println("Массив:");
    for(int i=0;i<a.length;i++)
        System.out.println(a[i]+" ");
    System.out.println();
}
T elemByInd(int i) throws ArrayIndexOutOfBoundsException
{
if (i<0||i>a.length)
    throw  new ArrayIndexOutOfBoundsException ("Выход за границы индекса");
return a[i];
}
//void  obr () - обратный порядок
void chast(int p) throws ArrayIndexOutOfBoundsException
{
if (p<0||p>a.length)
    throw  new ArrayIndexOutOfBoundsException ("Выход за границы индекса");
for (int i=0;i<p;i++)
        System.out.println(a[i]+" ");
}
}

public class Djen {

   
    public static void main(String[] args) {
        //работа с массивом целых
      Integer [] m1= {1,2,3,4,5};
      Mass<Integer> mi=new Mass<>(m1);
      try{
          System.out.println(mi.elemByInd(3));
          //mi.obr();
          mi.print();
          mi.chast(2);
      }
      catch (ArrayIndexOutOfBoundsException e)
      { System.out.println( e.getMessage());}
       //работа с массивом вещественных
      Double []m2={1.1,2.2,3.3,4.4,5.5};
      Mass <Double> md=new Mass<>(m2);
       try{
          System.out.println(md.elemByInd(3));
          //mi.obr();
          md.print();
          md.chast(2);
      }
        catch (ArrayIndexOutOfBoundsException e)
      { System.out.println( e.getMessage());}
        //работа с массивом строк
        String []m3={"123","234","345"};
      Mass <String> ms=new Mass<>(m3);
       try{
          System.out.println(ms.elemByInd(3));
          //mi.obr();
          ms.print();
          ms.chast(2);
      }
        catch (ArrayIndexOutOfBoundsException e)
      { System.out.println( e.getMessage());}
    }
    //задание-написать метод разворота массива, добавить ввод данных с клавиатуры
}
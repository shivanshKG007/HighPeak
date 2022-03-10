# HighPeak
Assignment
import java.io.*;
import java.util.*;
class Item {
  // global variable
  String name;
  int price;
// using .this operator to refernce the name and price intead creating new variables everytime
  public Item(String name, int price) {
    this.name = name;
    this.price = price;
  }
// returning the string in the below given fromat
  public String toString() { 
      return this.name + ": " + this.price;
  }
}

public class Main {
  public static void main(String[] args) throws Exception {
    FileInputStream fis=new FileInputStream("input.txt");  // input file      
    Scanner sc=new Scanner(fis); // using file operation in java
    int number_of_employees = Integer.parseInt(sc.nextLine().split(": ")[1]);
    sc.nextLine(); sc.nextLine(); sc.nextLine();
    // scanning the next line 
// converting the items and goodies list as array to access 
    ArrayList<Item> goodies_items = new ArrayList<Item>();

    while(sc.hasNextLine())  // breaking the while statement
    {
      String current[] = sc.nextLine().split(": ");
      goodies_items.add(new Item(current[0], Integer.parseInt(current[1])));// adding the new items as pos 0, 
    }
    sc.close(); // closing the scanner for further 
// sorting goodies items as array list item as 
    Collections.sort(goodies_items, new Comparator<Item>(){
      public int compare(Item a, Item b) { 
        return a.price - b.price; // finding the diffrence b/w the prices
      } 
    });

    int min_diff = goodies_items.get(goodies_items.size()-1).price; 
    // getting min_diff by size-1 for both goodies and price as well
    int min_index = 0; // define index value which is always start from 0
    for(int i=0;i<goodies_items.size()-number_of_employees+1;i++) { // to find the diffrence b/w the goodies item
      int diff = goodies_items.get(number_of_employees+i-1).price-goodies_items.get(i).price;
      // based on employee getting the goodies 
      // if diff obtained less than 0 then change it to the min diff as the obtained diff and min index is changed to the current position
      if(diff<=min_diff) {
        min_diff = diff;
        min_index = i;
      }
    }
    
    // for loop to write the minimum value using the index number 

    FileWriter fw = new FileWriter("output.txt"); // output file
    fw.write("The goodies selected for distribution are:\n\n");
    //choosing the items based on the number of employee
    for(int i=min_index;i<min_index + number_of_employees; i++) { // for loop using to find the goodies based on 
                                                                  // employee based on min index number 
      fw.write(goodies_items.get(i).toString() + "\n"); //write file in goodies items as string 
    }
     // for loop from min-index which is changed to i+ no of employees 
    // write item to file in .toString format
    fw.write("\nAnd the difference between the chosen goodie with highest price and the lowest price is: " + min_diff); // writing the minimum diffrence value
	  fw.close();  // closing file
  }
}

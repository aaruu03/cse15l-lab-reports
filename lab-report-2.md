# Lab Report 2

## Part 1
During lab last week, I completed the Simplest Search Engine task that we were assigned in lab. This was my code for the Engine:

```
class Handler implements URLHandler {
   // The one bit of state on the server: a number that will be manipulated by
   // various requests.
   int num = 0;
   ArrayList<String> list = new ArrayList<String>();
   public String handleRequest(URI url) {
       if(url.getPath().equals(“/")){
           return String.format("home!");
       }
       if(url.getPath().contains("/add")){
           String[] parameters = url.getQuery().split("=");
           if(parameters[0].equals("s")){
               list.add(parameters[1]);
               return String.format("query added!");
           }
       }
       else if(url.getPath().contains("/search")){
           String[] parameters = url.getQuery().split("=");
           String ret = "";
           if(parameters[0].equals("s")){
               String sub = parameters[1];
               for(int i = 0; i< list.size(); i++){
                   if(list.get(i).contains(sub)){
                       ret = ret + list.get(i) + " ";
                   }
               }
               return String.format("Search results: %s", ret);
           }
       }
       return "404 Not Found!";
   }
}

```

This was the search engine after opening the local host url:

![Image](lab2ss/home.png)

I then proceeded to add two different words (apple & pineapple) to the program that could later be queried:

![Image](lab2ss/apple.png)
![Image](lab2ss/pineapple.png)

For these, my code first checked if the url had the string "add" in the path. If it did, it entered the if-statement block and proceeded to get the parameters and split them into an array. When the user tries to add a word to the list, the word is 'stored' as the variable s in the string, so after checking the array that splits them up, we are able to see whether the first element of the array is s. This means that the next element in the array is the word we need to add, and thus after checking that, we add it to the list.

Finally, I tried searching for the substring "apple" which then returned both the words we added earlier since they contain the substring we searched for.

![Image](lab2ss/search.png)

For search, it was a a similar process where we checked if search existed in the path and then checked the parameters but this time instead of adding the word the user inputted to the list, we use it to search through our list using a loop and return all the words that contain the string the user chose.

## Part 2

The first file with a bug  was from the ArrayExamples.java.
This was the code from the first method.
```
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```
The failing output was that the array only flipped half of the array and left the original at the end. For example, if the input was an array {1, 2, 3, 4, 5} then the flipped one was {5, 4, 3, 4, 5}. This was because the loop went through all the indexes without keeping track of the original so the data was lost during the reversal.

To fix that, you would need to keep track of the original array using a temporary variable so that it would not overwrite the rest of the array. 

The second file's method is as follows:

```
static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index1 += 1;
    }
    return result;
  }
```
The failure inducing input was when I tried entering null as one of the arguments for the merge method. The symptom was the terminal/output giving an error to the user for the null value.

This was because the method did not have any measures to take against null values and it was not able to handle it correctly. To fix this, the method needs to have another if statement that checks against null statements and is able to catch it and let the user know instead of just failing.
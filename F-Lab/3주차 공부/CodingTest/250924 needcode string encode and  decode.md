```java
public String encode(List<String> strs) {  
  
    StringBuilder sb = new StringBuilder();  
  
    for (String item : strs) {  
        sb.append(item.length()+ "!" + item);  
    }  
    return  sb.toString();  
}  
  
public List<String> decode(String str) {  
  
    List<String> strs = new ArrayList<>();  
  
    int i = 0;   
    char[] arr = str.toCharArray();  
    while(i < arr.length) {  
        StringBuilder wordNumberString = new StringBuilder();  
        while(arr[i] != '!') {  
            wordNumberString.append(arr[i++]);  
        }  
        i++;  
        int wordLength = Integer.parseInt(wordNumberString.toString());  
        StringBuilder word = new StringBuilder();  
        for (int j = 0; j<wordLength; j++) {  
            word.append(arr[i++]);  
        }  
        strs.add(word.toString());  
    }  
  
    return  strs;  
}
```

솔직히 생각하다가 도저히 답이 안나서 답지를 보고 어떻게 진행해야 하는지 파악했다.
다행히 작성한 코드는 한번에 통과해서 즐거웠다.

꽤나 흥미로운 문제.

첫번째는 단순히 문자로만 String 을 구분할 것인가 ? 라고 했을때 
counter case 가 단순히 문자 그 자체가 되므로
단어의 숫자 + 문자를 가지고
무조건 숫자 다음의 문자면 그땐 단어의 숫자가 끝난것으로 생각하고
다음 단어를 읽는 방법을 고안해내는
그 사고방식을 배워야 겠다. 처음보는 문제여도.




```java
import java.io.BufferedReader;  
import java.io.BufferedWriter;  
import java.io.InputStreamReader;  
import java.io.OutputStreamWriter;  
import java.util.ArrayList;  
import java.util.StringTokenizer;  
  
public class Main {  
  
    static BufferedReader br;  
    static BufferedWriter bw;  
    static StringTokenizer st;  
    static StringBuilder sb;  
    static int answer;  
    static char[] chars = {'B', 'W'};  
    static char[][] mat;  
    static int m;  
    static int n;  
    static int min;  
    static int currentCount;  
    static char[][] tmp;  
    static boolean changed;  
  
        public static void main(String[] args) throws Exception {  
  
            br = new BufferedReader(new InputStreamReader(System.in));  
            bw = new BufferedWriter(new OutputStreamWriter(System.out));  
            sb = new StringBuilder();  
            st = new StringTokenizer(br.readLine());  
  
            n = Integer.parseInt(st.nextToken());  
            m = Integer.parseInt(st.nextToken());  
            mat = new char[n][m];  
            for (int i=0; i<n; i++) {  
                mat[i] = br.readLine().toCharArray();  
            }  
  
            tmp = new char[8][8];  
            min = Integer.MAX_VALUE;  
  
            for (int i=0; i<=n-8; i++) {  
                for (int j=0; j<=m-8; j++) {  
                    for (int k=0; k<2; k++) {  
                        int result = calculateNumbers (chars[k], i, j);  
                        if (result < min) min = result;  
                    }  
                }  
            }  
  
            sb.append(min);  
            bw.write(sb.toString());  
            bw.close();  
        }  
  
        public static int calculateNumbers (char first, int x, int y) {  
            currentCount = 0;  
            changed = false;  
            int blocksNeeded = 0;  
  
            // init charCount  
            int charCount = 0;  
            if (first == chars[1]) charCount++;  
  
            // copy matrix  
            for (int i=0; i<8; i++) {  
                for (int j=0; j<8; j++){  
                    tmp[i][j] = mat[x+i][y+j];  
                }  
            }  
  
            for (int i=0; i<8; i++) {  
                // we need to change first element  
                if (i > 0 && (tmp[i][0] == tmp[i-1][0])) {  
                    tmp[i][0] = chars[(charCount)%2];  
                    currentCount++;  
                }  
                blocksNeeded += calculateNumbersOfLines(chars[(charCount++)%2], i);  
            }  
            return blocksNeeded;  
        }  
  
        public static int calculateNumbersOfLines (char first, int x) {  
            int charCount = 0;  
            int blocksNeeded = 0;  
            if (first == chars[1]) charCount++;  
  
            for (int i=1; i<8; i++) {  
                if (chars[(charCount%2)] == tmp[x][i]) {  
                    tmp[x][i] = chars[(charCount+1)%2];  
                    blocksNeeded++;  
                    // paint 를 하면서 가야되네...  
                }  
                charCount++;  
            }  
            return blocksNeeded;  
        }  
    }
```

나중에 다시 봐야지.
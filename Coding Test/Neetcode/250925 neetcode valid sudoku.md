```java
  
public class Solution {  
  
    public static boolean [] check = new boolean[10];  
  
    public boolean isValidSudoku(char[][] board) {  
  
        for (int i=0; i<9; i++) {  
            resetCheck();  
            if (!checkRow(board, i)) return false;  
            resetCheck();  
            if (!checkColumn(board, i)) return false;  
        }  
  
        for (int i=0; i<3; i++) {  
            for (int j=0; j<3; j++) {  
                resetCheck();  
                if (!checkBox(board, i*3, j*3)) return false;  
            }  
        }  
  
        return true;  
    }  
  
    public void resetCheck () {  
        for (int i=0; i<10; i++) {  
            check[i] = false;  
        }  
    }  
  
    public boolean checkRow (char[][] board, int row) {  
        for (int i=0; i<9; i++) {  
            if (board[row][i] == '.') continue;  
            int ch = board[row][i] - '0';  
            if (check[ch ]) return false;  
                    else check[ch] = true;  
            }  
        return true;  
    }  
  
    public boolean checkColumn (char[][] board, int column) {  
        for (int i=0; i<9; i++) {  
            if (board[i][column] == '.') continue;  
            int ch = board[i][column] - '0';  
            if (check[ch]) return false;  
                    else check[ch] = true;  
            }  
        return true;  
    }  
  
    public boolean checkBox (char[][] board, int x, int y) {  
        for (int i=x; i<x+3; i++) {  
            for (int j=y; j<y+3; j++) {  
                if (board[i][j] == '.') continue;  
                int ch = board[i][j] - '0';  
                if (check[ch]) return false;  
                else check[ch] = true;  
            }  
        }  
  
        return true;  
    }  
}
```

바보처럼 거의 다 짜놓고
각 check 가 false 일때 isValidSudoku 에서 false 를 리턴하지 않는 바보짓을 했다.
# Find-the-String-with-LCP.
leetcode problem No:2573

import java.util.*;

class Solution {
    public String findTheString(int[][] lcp) {
        int n = lcp.length;

        // Step 0: basic validation
        for (int i = 0; i < n; i++) {
            if (lcp[i][i] != n - i) return "";
        }

        char[] res = new char[n];
        Arrays.fill(res, '#');

        char ch = 'a';

        // Step 1: assign characters
        for (int i = 0; i < n; i++) {
            if (res[i] == '#') {
                if (ch > 'z') return "";

                for (int j = i; j < n; j++) {
                    if (lcp[i][j] > 0) {
                        res[j] = ch;
                    }
                }
                ch++;
            }
        }

        // Step 2: recompute LCP
        int[][] calc = new int[n][n];

        for (int i = n - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if (res[i] == res[j]) {
                    if (i == n - 1 || j == n - 1) {
                        calc[i][j] = 1;
                    } else {
                        calc[i][j] = 1 + calc[i + 1][j + 1];
                    }
                } else {
                    calc[i][j] = 0;
                }
            }
        }

        // Step 3: validate
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (calc[i][j] != lcp[i][j]) {
                    return "";
                }
            }
        }

        return new String(res);
    }
}
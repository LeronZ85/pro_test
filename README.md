# pro_test


```

import java.util.ArrayList;
import java.util.List;

import static junit.framework.TestCase.fail;

public class Main {

    @org.junit.Test
    public void testIp2Int() {
        assert ip2Int(" 172  .168 .5.1 ") == 2896692481L;
        assert ip2Int("   172  .  168 .  5.1 ") == 2896692481L;
        assert ip2Int(" 000  .00 .15.11 ") == 15 * 256 + 11;
        try {
            ip2Int("17 2.168.5.1");
            fail();
        } catch (RuntimeException e) {
        }
        try {
            ip2Int("172. 16    8.  5  .1");
            fail();
        } catch (RuntimeException e) {
        }
        try {
            ip2Int("  172  . 16 8.  5  .1");
            fail();
        } catch (RuntimeException e) {
        }
    }

    public static long ip2Int(String ipStr) {
        int firstIntIndex = -1, lastIntIndex = -1, faster = 0;
        int temp = 0;
        int preSpaceIndex = -1;
        List<Integer> list = new ArrayList<>();

        while (faster < ipStr.length()) {
            if (isSpot(ipStr, faster)) {
                checkSpace(preSpaceIndex, firstIntIndex, lastIntIndex);
                list.add(temp);
                temp = 0;
                firstIntIndex = -1;
                lastIntIndex = -1;
                preSpaceIndex = -1;
            }
            if (isInteger(ipStr, faster)) {
                temp = temp * 10 + genInt(ipStr, faster);
                if (firstIntIndex == -1) {
                    firstIntIndex = faster;
                }
                lastIntIndex = faster;
            }
            if (isBlank(ipStr, faster)) {
                preSpaceIndex = faster;
            }
            faster++;
        }
        list.add(temp);
        long result = 0;
        for (int data : checkNum(list)) {
            result = result * 256 + data;
        }
        return result;
    }

    /**
     * IPV4 最多4组数字
     * @param list
     */
    private static List<Integer> checkNum(List<Integer> list) {
        if (list.size() != 4) {
            throw new RuntimeException();
        }
        return list;
    }

    /**
     * 17 2.168.5.1 不是合法的IP地址
     * @param preSpaceIndex
     * @param firstIntIndex
     * @param lastIntIndex
     */
    private static void checkSpace(int preSpaceIndex, int firstIntIndex, int lastIntIndex) {
        if (isUnValid(preSpaceIndex, firstIntIndex, lastIntIndex)) {
            throw new RuntimeException();
        }
    }

    private static boolean isUnValid(int preSpaceIndex, int firstIntIndex, int lastIntIndex) {
        return firstIntIndex != -1 && preSpaceIndex > firstIntIndex && preSpaceIndex < lastIntIndex;
    }

    private static Integer genInt(String ipStr, int faster) {
        return ipStr.charAt(faster) - '0';
    }

    private static Character SPOT = '.';

    private static Character BLANK = ' ';

    private static boolean isSpot(String ipStr, int faster) {
        return (SPOT).equals(ipStr.charAt(faster));
    }

    private static boolean isBlank(String ipStr, int faster) {
        return (BLANK).equals(ipStr.charAt(faster));
    }

    private static boolean isInteger(String ipStr, int faster) {
        return ipStr.charAt(faster) >= '0' && ipStr.charAt(faster) <= '9';
    }

}
```

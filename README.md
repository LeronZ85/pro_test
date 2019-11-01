# Convert IPv4 address
>       Programming Question:
        Convert an IPv4 address in the format of null-terminated C string into a 32-bit integer.
        For example, given an IP address “172.168.5.1”, the output should be a 32-bit integer
        with “172” as the highest order 8 bit, 168 as the second highest order 8 bit, 5 as the
        second lowest order 8 bit, and 1 as the lowest order 8 bit. That is,
        "172.168.5.1" => 2896692481
        Requirements:
        1. You can only iterate the string once.
        2. You should handle spaces correctly: a string with spaces between a digit and a dot is
        a valid input; while a string with spaces between two digits is not.
        "172[Space].[Space]168.5.1" is a valid input. Should process the output normally.
        "1[Space]72.168.5.1" is not a valid input. Should report an error.
        3. Please provide unit tests.

```


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
        long result = 0;
        while (faster < ipStr.length()) {
            if (isSpot(ipStr, faster)) {
                checkSpace(preSpaceIndex, firstIntIndex, lastIntIndex);
                result = result * 256 + temp;
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
        result = result * 256 + temp;
        return result;
    }

    /**
     * IPV4 最多4组数字
     *
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
     *
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

    private static boolean isSpot(String ipStr, int faster) {
        Character SPOT = '.';
        return (SPOT).equals(ipStr.charAt(faster));
    }

    private static boolean isBlank(String ipStr, int faster) {
        Character BLANK = ' ';
        return (BLANK).equals(ipStr.charAt(faster));
    }

    private static boolean isInteger(String ipStr, int faster) {
        return ipStr.charAt(faster) >= '0' && ipStr.charAt(faster) <= '9';
    }


}

```

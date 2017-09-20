# Leak report

_Use this document to describe whatever memory leaks you find in `clean_whitespace.c` and how you might fix them. You should also probably remove this explanatory text._
The following message is returned when running valgrind --leak-check=full ./check_whitespace
==21325== 
==21325== HEAP SUMMARY:
==21325==     in use at exit: 46 bytes in 6 blocks
==21325==   total heap usage: 7 allocs, 1 frees, 1,070 bytes allocated
==21325== 
==21325== 46 bytes in 6 blocks are definitely lost in loss record 1 of 1
==21325==    at 0x4C2FA50: calloc (vg_replace_malloc.c:711)
==21325==    by 0x400678: strip (check_whitespace.c:41)
==21325==    by 0x4006E3: is_clean (check_whitespace.c:62)
==21325==    by 0x40076B: main (check_whitespace.c:87)
==21325== 
==21325== LEAK SUMMARY:
==21325==    definitely lost: 46 bytes in 6 blocks
==21325==    indirectly lost: 0 bytes in 0 blocks
==21325==      possibly lost: 0 bytes in 0 blocks
==21325==    still reachable: 0 bytes in 0 blocks
==21325==         suppressed: 0 bytes in 0 blocks
==21325== 
==21325== For counts of detected and suppressed errors, rerun with: -v
==21325== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
It seems like the memory leak is occuring. 46 bytes are lost. The leak is occuring in strip since is_cleaned uses strip. In the actual code strip contains the line: result = calloc(size-num_spaces+1, sizeof(char));. Result is never being cleared, so the memory is never made available for future use. The function is_cleaned contains a pointer for the result of stripped, so freeing up the pointer in is_cleaned should fix the leakage.

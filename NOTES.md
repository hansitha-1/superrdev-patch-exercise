
Summary of changes
1. Fixed SQL operator precedence (adding parentheses around the OR) in TaskRepository.java. It allowed searching for archived tasks and tasks with wrong status. The same fix applied to db/oracle/task_search_package.sql and db/queries/search_tasks.sql
2. Removed artificial Thread.sleep in TaskController.java which made quick queries slower than the slow ones
3. useTasks.js now ignores late responses (have cleanup flag) and does not keep loading spinner after request failed
4. Invalid status now returns 400 instead of 500
5. page and pageSize are validated (return 400 on invalid input)
6. % and _ in a search query are escaped to match them literally
7. Search and status changes now reset the page to 1 in App.jsx. Before, a stale page number showed "No tasks found" even when results existed

What I decided not to change
Pagination is done in memory in the controller (all matches are fetched, then paged). That is acceptable for this small table. I also did not add debounce to the search box, since the cleanup flag already stops stale results. Results are sorted newest first (created_at DESC). I treated that as intended behaviour.

Biggest remaining risk
The in-memory pagination will not scale for tables with large data sets. Another thing is that I could not test the Oracle package directly, as it runs only on the database server.The LIKE statement in the package does not have an ESCAPE clause, so it might not behave as expected.

Tools and techniques used
I used Claude as a guide. It pointed me to the files, explained each root cause, and gave me the code for each fix. I reproduced each bug, applied and tested every change myself, and wrote the handwritten notes.
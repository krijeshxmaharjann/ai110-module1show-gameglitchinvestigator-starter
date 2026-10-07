# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

When I first ran the game, it displayed a number guessing interface with a Developer Debug Info section that showed the secret number. I noticed that the Higher/Lower hints were backwards, so the game could tell me to go higher when my guess was already higher than the secret. I also found a problem with how the secret value was handled because the code changed it between an integer and a string during different attempts. Another issue I found was that the score update function existed but was not being called by the main application.

**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input                         | Expected Behavior                                 | Actual Behavior                                                                                                                                                   | Console Output / Error                           |
| ----------------------------- | ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| Guess 70 when secret is 50    | Game should say "Go LOWER"                        | Game gave the wrong Higher/Lower direction.                                                                                                                       | No console error; incorrect game behavior.       |
| Guess 40 when secret is 50    | Game should say "Go HIGHER"                       | Game gave the wrong Higher/Lower direction.                                                                                                                       | No console error; incorrect game behavior.       |
| Submit guesses multiple times | Secret should remain the same during the game     | The secret value was stored in session state, but the code changed its type between an integer and a string on different attempts, causing incorrect comparisons. | No console error; incorrect comparison behavior. |
| Start a new game              | Attempts, score, history, and secret should reset | The New Game function needed to reset the game state.                                                                                                             | No console error; incorrect state behavior.      |


---

## 2. How did you use AI as a teammate?

I used ChatGPT as my main AI coding teammate. One useful suggestion was to store the game information in Streamlit's st.session_state so values such as the secret number, attempts, score, and history persist when Streamlit reruns the application. I verified the changes by running the game repeatedly, checking the Developer Debug Info, submitting guesses, and using the New Game button.

One suggestion I changed rather than accepting exactly as written was the implementation of check_guess(). An initial version returned both the outcome and a message, but the pytest tests expected only an outcome such as "Win", "Too High", or "Too Low". I changed check_guess() to return only the outcome and handled the user-facing hint messages separately in app.py. I verified this decision by running pytest and getting all three tests to pass.

---

## 3. Debugging and testing your fixes

I decided a bug was fixed when the behavior matched what I expected and the automated tests passed. I manually tested the game by looking at the secret in Developer Debug Info and entering guesses that were higher, lower, and equal to the secret. I also ran py -m pytest, which collected three tests and resulted in 3 passed in 0.06s. AI helped me understand what the tests were checking and helped me interpret the pytest failure when check_guess() returned a tuple instead of the expected outcome string.

---

## 4. What did you learn about Streamlit and state?

I learned that Streamlit reruns the Python script when the user interacts with widgets such as buttons. This means normal Python variables can be recreated during each rerun, so information that needs to survive between interactions should be stored in st.session_state. I think of session state like a small storage area for the current game: the script can rerun, but values such as the secret number, score, attempts, and history can remain available.

---

## 5. Looking ahead: your developer habits

One habit I want to reuse is testing small pieces of code instead of only testing the entire application at the end. Running pytest helped me catch the fact that my check_guess() function returned the wrong type, even though the logic itself looked reasonable.

Next time I work with AI on a coding task, I want to understand and verify each suggested change instead of copying code without checking how it fits with the existing project. This project showed me that AI-generated code can look correct while still containing logic, state, or integration bugs. I now see AI as a coding teammate that can help me debug and explain problems, but whose suggestions still need to be tested and reviewed.
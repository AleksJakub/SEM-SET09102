# Testing

## Testing CreateNewChallenge Method for Easy Difficulty

**Purpose of the Code:**
This method generates a new word based on the selected game type.

```
[Fact]
public void Test_CreateNewChallenge_ForWordInitialization()
{
    // Arrange
    var gamePage = new GamePage("Easy");

    // Act
    gamePage.CreateNewChallenge();

    // Assert
    Assert.NotNull(gamePage.Word);
    Assert.NotEmpty(gamePage.Word);
}
```
**Explanation of the Test:**
This test method simulates the scenario where a user starts a new game in 'Easy' mode. It checks if the CreateNewChallenge method sets the Word property with a non-null and non-empty string after creating a new challenge. This ensures that players always have a valid word to guess.

**Importance of Testing:**
By testing the word initialization process, we guarantee that our game starts with a valid challenge, enhancing the user experience. This validation is fundamental for maintaining the game's integrity and preventing unexpected issues during gameplay.

**Limitations:**
Future improvements could include additional checks for word relevance to the chosen difficulty level.

  

## Testing SelectWord Method for Medium Difficulty

**Purpose of the Code:**
Word selection logic for the "Medium" difficulty level in the GamePage class.

```
[Fact]
public void Test_SelectWord_ForMediumDifficulty()
{
    // Arrange
    var gamePage = new GamePage("Medium");

    // Act
    string selectedWord = gamePage.SelectWord("Medium");

    // Assert
    Assert.True(selectedWord.Length >= 7 && selectedWord.Length < 10);
}
```

**Explanation of the Test:**
This test validates the behavior when a user chooses the 'Medium' difficulty level. It checks if the SelectWord method returns a word with a length between 7 and 9 characters. This ensures that players in the "Medium" mode face challenges of medium complexity.

**Importance of Testing:**
By confirming that the selected word aligns with the specified difficulty level, we maintain a balanced gameplay experience. This validation is important for ensuring that players are presented with appropriately challenging words for their chosen difficulty setting.

**Limitations:**
Future improvements could include additional checks for word relevance to the chosen difficulty level.


# Project work 4

## Issue description
As an UNDAC Analyst, I want to view the status of current and completed operations so that I can evaluate the effectiveness of the mission.

## Code Description
```
// This method updates the status of the operationstatus
public async void OnChange(object sender, EventArgs e)
{
    if (string.IsNullOrWhiteSpace(DetailsEntry.Text) | string.IsNullOrWhiteSpace(ProgressEntry.Text))
    {
        await DisplayAlert("ERROR", "Enter valid Details or Progress", "OK");
        return;
    }

    operationstatus.Details = DetailsEntry.Text;
    operationstatus.Progress = ProgressEntry.Text;
    await _operationstatusRepo.SaveAsync(operationstatus);

    if (PickerStatus.SelectedIndex >= 0)
    {
        operationstatus.Status = (string)PickerStatus.SelectedItem;

        await _operationstatusRepo.SaveAsync(operationstatus);
        await Navigation.PopAsync();
    }
}
```

Error Handling: The method includes error handling by checking if the DetailsEntry and ProgressEntry are empty. If so, it displays an error alert. This is good practice for user input validation.
Consistent Naming: Variable names (DetailsEntry, ProgressEntry, PickerStatus) follow a consistent naming convention, increasing readabiliity.
Conditional Flow: The method uses conditional statements to handle different scenarios. For example, it checks if the PickerStatus has an index before updating the status and going back. This helps ensure the method executes only when necessary.

## Test Code
```
[Test]
public async Task DeleteOperationStatusTestAsync()
{
    var testOperationStatus = new OperationStatus
    {
        Status = "Completed",
        Progress = "resources allocated",
        Details = "systems down, need fixed immediately"
    };
    await repo.SaveAsync(testOperationStatus);

    await repo.DeleteAsync(await repo.GetAsync(testOperationStatus.Id));
    var readOperationStatus2 = await repo.GetAsync(testOperationStatus.Id);

    Assert.That(readOperationStatus2, Is.Null, " Test OperationStatus was not deleted");
}
```

The provided tests thoroughly check how well the OperationStatus class works in the Undac app. First, it sets up a test database and repository and cleans up afterward. Each test method, like AddOperationStatusTestAsync, checks a specific part of the repository's job, from adding and deleting OperationStatus instances to getting lists of them. Clear and simple checks, along with using async/await for smooth testing, make the tests reliable without causing any issues. These tests make sure the OperationStatusRepository in the Undac app can handle adding, deleting, and retrieving OperationStatus instances as expected.

## Review Requests
During the code review, feedback was provided on the need for better error messages in case of test failures and more descriptive test method names. Also, there was a suggestion to include assertions for each property of the OperationStatus class. In response, error messages were improved, test method names were made more descriptive, and assertions for each property were added.

## Reviewer Requests
The main issue identified in the code review was the need for more informative error messages and assertions in the tests. This was addressed by refining error messages and adding detailed assertions to ensure a clearer understanding of test failures. However, regardless of this I can see massive progress being made from the whole team if compared to the first couple of weeks.

## Reflection

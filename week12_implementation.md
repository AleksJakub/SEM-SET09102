# Project work 4

Week 10 is the fourth in a series in which the goal is to improve your 
personal software engineering practice. Your portfolio entry has the same general content
as last week's, including:

* A descriptive summary of the issue that you worked on.
* Snippets from your code with commentary showing how you have used good software design 
  practice.
* A descriptive summary of the test code that you have written.
* A reflective summary of any changes that were requested during the code review along 
  with your fixes.
* A descriptive summary of any issues you found with the code that you were asked to review.
* A general reflective section that identifies, for example,
  * New things you have realised this week
  * Common problems that can arise in a team development situation
  * How your practice compares to other people's
  * etc.

Be sure to include links to the original items in the team's GitHub repository.

In the reflective sections this week, you should highlight ways that you persona practice
has improved as before. It would also be good to reflect on any improvements that have
been made to the agreed team workflow and related procedures. Are things working
better than they were? What further improvements could be made in the future?


## Issue description
As an UNDAC Analyst, I want to view the status of current and completed operations so that I can evaluate the effectiveness of the mission.

## Code Description
```
// This method loads all the Operation Status into the UI
private async void LoadAllOperationStatus()
{
    var operationstatus = await operationstatusRepo.GetAllAsync();
    allOperationStatus = operationstatus;
    OperationStatusListView.ItemsSource = allOperationStatus;
}
```
Asynchronous Programming: The LoadAllOperationStatus method is marked as async, indicating its asynchronous nature. This is crucial for preventing UI blocking during potentially time-consuming operations like fetching data.
Data Binding: The method efficiently uses data binding (OperationStatusListView.ItemsSource) to link the UI with the retrieved operation status data. This adheres to the MVVM (Model-View-ViewModel) pattern, promoting separation of concerns. 

```
public async void OnChange(object sender, EventArgs e)
{
    if (string.IsNullOrWhiteSpace(NeedTypeEntry.Text) | string.IsNullOrWhiteSpace(LocationEntry.Text))
    {
        await DisplayAlert("ERROR", "Enter a valid Need Type or Location", "OK");
        return;
    }

    need.Location = LocationEntry.Text;
    need.NeedType = NeedTypeEntry.Text;

    await _needsRepo.SaveAsync(need);

    if (PickerUrgency.SelectedIndex >= 0)
    {
        need.Urgency = (string)PickerUrgency.SelectedItem;

        await _needsRepo.SaveAsync(need);
        await Navigation.PopAsync();
    }
}

```
Error Handling: The method starts with error handling to ensure that both NeedTypeEntry and LocationEntry are not empty. This is a good practice for validating user input and preventing invalid data from being processed.
Asynchronous Programming: The method uses async/await for asynchronous operations, specifically when calling _needsRepo.SaveAsync(need). This ensures that the UI remains responsive while waiting for the asynchronous operation to complete.

## Test Code

## Review Requests

## Reviewer Requests

## Reflection

## Issue

Since the issues from week 3 and week 8 have been completed and the issues from week 9 haven't been released with full requirements, then this week I decided to go back over everything i done in the previous weeks and made adjustments.

## Code Description
```
/// <summary>
/// Handles the event when the "Add Room Use Type" button is clicked.
/// </summary>
/// <param name="sender">The sender of the event.</param>
/// <param name="e">The event arguments.</param>
private void ClkAddBtn(object sender, EventArgs e)
{
    Navigation.PushAsync(new AddRoomUseTypeView());
}

/// <summary>
/// Handles the event when the "Edit Room Use Type" button is clicked.
/// </summary>
/// <param name="sender">The sender of the event.</param>
/// <param name="e">The event arguments.</param>
private void ClkEdtBtn(object sender, EventArgs e)
{
    if (ListViewRoomUseType.SelectedItem is RoomUseTypes roomusetype)
    {
        Navigation.PushAsync(new EditRoomUseTypeView((RoomUseTypes)ListViewRoomUseType.SelectedItem));
    }
}

/// <summary>
/// Handles the event when the "Delete Room Use Type" button is clicked.
/// </summary>
/// <param name="sender">The sender of the event.</param>
/// <param name="e">The event arguments.</param>
private async void ClkRmvBtn(object sender, EventArgs e)
{
    if (ListViewRoomUseType.SelectedItem is RoomUseTypes roomusetype)
    {
        await App.Database.DeleteItemAsync((RoomUseTypes)ListViewRoomUseType.SelectedItem);
        LoadRoomUseTypes();
    }

}
```
Here I added Doxygen comments to the code as they were missing from the CRUD Room Use Types Feature. Adding Doxygen comments to the CRUD Room Use Types Feature enhances code readability, aids in understanding the purpose and functionality.

```
var existingRoomUseType = await App.Database.GetItemAsync(NameEntry.Text);
if (existingRoomUseType != null)
{
	await DisplayAlert("ERROR", "Room Use Type already exists", "OK");
    return;
}
```
In the same branch i went through the code and remove the other database i made to make this code function and replaced it with the database template that is being used in all branches within the team code. This is to improve the readability for anyone reading the code as it'll match other branches and will also allow for consistancy in the overall program which will decrease the resources used and increase the efficiency.

```
    private async void LoadAllSecurityAlerts()
    {
        try
        {
            var securityalerts = await securityalertsRepo.GetAllAsync();
            allSecurityAlerts = securityalerts;
            SecurityAlertsListView.ItemsSource = allSecurityAlerts;
        }
        catch (Exception ex)
        {
            // Log the exception
            Console.WriteLine($"Error loading security alerts: {ex.Message}");
            await DisplayAlert("Error", "Failed to load security alerts. Please try again later.", "OK");

        }
    }
```
within the Security Alerts branch I added the try-catch block which effectively handles exceptions that might occur during the operation, preventing the application from crashing. It logs the exception for debugging purposes. This contributes to the robustness and maintainability of the code.

## Testing

```
 [Test]
 public async Task DeleteRoomUseTypeAsync()
 {
     var roomusetypeInfo = new RoomUseType
     {
         Id  = 1,
         roomUseType = "Kitchen"
     };

     await _roomusetypeRepository.SaveAsync(roomusetypeInfo);

     await _roomusetypeRepository.DeleteAsync(await _roomusetypeRepository.GetAsync(roomusetypeInfo.Id));

     RoomUseType deletedRoomUseType = await _roomusetypeRepository.GetAsync(roomusetypeInfo.Id);

     Assert.That(deletedRoomUseType, Is.Null, "Room Use Type was not deleted");
 }
```
In the Room Use Type feature branch i added to code to ensure the all of the CRUD functionality works as intended

## Changes made

The changes made above in the code description were a result of all previous requests made on my PR's during the course of this module

## Changes Requested

while reviewing a Pull Request i found the the User Intereface in the code was not following the template that is supposed to be used for certain branches so i requested that be added in rather than including a different UI. I also requested the developer resolve the merge conflicts that appeared for 3 of his files as this might cause issues with merging the branch. Lastly I requested to added Doxygen comments to certain parts of the code and to add Exception handling however that is not required but is good code practice.

## Reflection

This week, I picked up on the idea that keeping things consistent in our code is a big deal. I decided to swap out a specific database with a more general template that everyone's using in different branches. It not only made the code easier to read but also made the whole program look more put together. Also, I started adding try-catch blocks in the code to handle errors better.

Working in a team has its challenges. For example, I noticed that our user interface wasn't sticking to the plan in one of the branches. So, I asked for it to match the standard template we've got going. It just made me think about how crucial it is for everyone on the team to follow the same rules. Consistency is key, especially when it comes to making our code easy to understand and work with.

https://github.com/xinjoonha/SET09102_PURPLE/tree/feature/view-security-alerts/Undac 


# Documentation

## six rules of clean code

### KISS
The KISS (Keep it Stupid Simple) principle promotes design simplicity. It proposes that system and software designs be kept as straightforward as can be in order to prevent unneeded complexity. Simpler code is easier to read, debug, and maintain, resulting in more robust and maintainable software.
```
public async void AddUpdateRoomUseType()
        {
            int response = -1;

            if(RoomUseTypeDetail.ID > 0) 
            {
                response = await _roomUseTypeService.UpdateRoomUseType(RoomUseTypeDetail);
            }
            else
            {
                response = await _roomUseTypeService.AddRoomUseType(new Models.RoomUseTypeModel
                {
                    ID = RoomUseTypeDetail.ID,
                    Name = RoomUseTypeDetail.Name,
                    RoomUseType = RoomUseTypeDetail.RoomUseType
                });
            }
```
The code checks whether RoomUseTypeDetail.ID is greater than 0 and then either updates or adds a new RoomUseType accordingly. The logic is simple and easy to understand.

### Comments
Developers offer explanation to the code as comments. They are intended to make specific code segments' intentions, logic, or behaviours easier to understand. Although comments are often useful, clean code encourages programmers to build self-explanatory, readable code without relying too heavily on comments. The need for lengthy comments is reduced by clear and expressive code.
```
if (response > 0)
{
    // If the response from the operation is greater than 0, it means the record was successfully added.
    await Shell.Current.DisplayAlert("Record Added", "Record Added to Room Use Type Table", "OK");
}
else
{
    // If the response is not greater than 0, it indicates an error or failure during the addition operation.
    // Display an alert to notify the user about the issue.
    await Shell.Current.DisplayAlert("Heads up!", "Something went wrong", "OK");
}
```
By adding these comments, the code becomes more self-explanatory, making it easier for other developers to understand the logic and purpose behind these conditional statements.

### Meaningful Names
It is essential to give variables, functions, classes, and other code objects meaningful and descriptive names. Meaningful names express the code's goal, making it more straightforward for other developers (and your future self) to grasp the purpose of various areas of the codebase. Descriptive names lead to better readability and maintenance.
```
public class RoomUseTypeModel
    {
        [PrimaryKey, AutoIncrement]
        public int ID { get; set; }
        public string Name { get; set; }
        public string RoomUseType { get; set; }
    }
```
ID: Represents the unique identifier of the object. The name is concise and meaningful.  
Name: Represents the name property. It's clear and easy to understand.  
RoomUseType: Represents the type of room use. The name is descriptive enough to convey its purpose.  

### DRY
The DRY(Don't Repeat Yourself) principle highlights the significance of reuseable code. It implies that identically functioning code sections shouldn't be repeated throughout the original code. To avoid duplication, such logic should be abstracted into functions, classes, or modules. Developers may retain consistency, reduce errors, and make code simpler to update and maintain by adhering to DRY.
```
if (_dbConnection == null)
            {
                string dbPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "RoomUseType.db3");
                _dbConnection = new SQLiteAsyncConnection(dbPath);
                await _dbConnection.CreateTableAsync<RoomUseTypeModel>();
            }
```
The provided code snippet follows the DRY principle by initializing the _dbConnection variable and creating a database table only if _dbConnection is null. This approach prevents redundant database connections and table creations, ensuring efficient and non-repetitive code execution.

### Standard Conventions
Consistent coding practices and styles are referred to as "standard conventions" within a team of developers or industry. Code readability and maintainability are improved by following consistent naming standards, indentation rules, and other formatting patterns. Following established conventions makes ensuring that the codebase has a consistent look and is simple for all team members to understand.
```
public partial class RoomUseTypeListPage : ContentPage
{
	private RoomUseTypeListPageViewModel _viewMode;
	public RoomUseTypeListPage(RoomUseTypeListPageViewModel viewModel)
	{
		InitializeComponent();
		_viewMode = viewModel;
		this.BindingContext = viewModel;
	}
```
 Class names like RoomUseTypeListPage follow PascalCase, where the first letter of each word is capitalized. Private member variables like _viewMode use camelCase, starting with a lowercase letter. Constructor names, such as RoomUseTypeListPage, match the class name, ensuring consistency and readability in the codebase. The code also follows the correct use of whitespace and indentation. These conventions enhance code clarity and maintainability for developers working on the project. 

### Functions
Blocks of code called functions, usually referred to as methods or procedures, are created to carry out particular duties. Functions in clean code have to be brief, concentrated on a single task, and have obvious inputs and outputs. Effectively constructed functions improve readability and make testing and debugging simpler. Clean and maintainable code is produced by adhering to the Single Responsibility Principle (SRP) and making sure functions do one thing very well.
```
public async void GetRoomUseTypeList()
        {
            var roomUseTypeList = await _roomUseTypeService.GetRoomUseTypeList();
            if(roomUseTypeList?.Count > 0 ) 
            {
                RoomUseTypes.Clear();
                foreach(var roomUseType in roomUseTypeList)
                {
                    RoomUseTypes.Add(roomUseType);
                }
            }
        }
```
GetRoomUseTypeList(): This function is focused on a single responsibility, fetching room use types from the service and populating the RoomUseTypes collection. It adheres to the Single Responsibility Principle.


**COULD NOT GET DOXYGEN TO WORK** (watched many videos but references provided did not have much useful information on how to get the updated code comments

```
protected override void OnAppearing()
    {
        base.OnAppearing();
        LoadRoomUseTypes();
    }
```
Here i removed the need for comments by having a meaningful function name

```
if (string.IsNullOrWhiteSpace(NameEntry.Text))
		{
			await DisplayAlert("ERROR", "Enter a valid Room Use Type", "OK");
			return;
		}

		var existingRoomUseType = await App.Database.GetItemAsync(NameEntry.Text);
		if (existingRoomUseType != null)
		{
			await DisplayAlert("ERROR", "Room Use Type already exists", "OK");
            return;
```
Simple code, meaningful names and obvious intentions meantioned in the DisplayAlert so the code is eaasy to understand without the need for comments

```
public class RoomUseTypes
    {
        [PrimaryKey, AutoIncrement]
        public int Id { get; set; }
        public string roomUseType{ get; set; }
    }
```
Meaningful Class and Variable names remove the need for comments


 

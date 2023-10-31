# Project work 1

## Issue
In this project, my role as the UNDAC Team Leader was to establish a process for requesting volunteers based on specific criteria such as skills, geographical location, and status. This initiative aimed to ensure that our team had the necessary capacity and skills required for our missions.

## Code

```
public void FilterVolunteers(object sender, EventArgs e)
{
    var filteredVolunteers = allVolunteers;

    if (!string.IsNullOrWhiteSpace(SkillSearchBar.Text))
    {
        string skillFilter = SkillSearchBar.Text.ToLower();
        filteredVolunteers = filteredVolunteers
            .Where(volunteer => volunteer.Skill.ToLower().Contains(skillFilter)).ToList();
    }

    VolunteersListView.ItemsSource = filteredVolunteers;
}
```
In this snippet, I've applied the Single Responsibility Principle (SRP) by creating a dedicated method FilterVolunteers. This method is responsible for filtering volunteers based on search criteria, adhering to the principle of encapsulating a single functionality within a method.

```
private async void LoadAllVolunteers()
{
    var volunteers = await volunteersRepo.GetAllAsync();
    allVolunteers = volunteers;
    VolunteersListView.ItemsSource = allVolunteers;
}
```
The snippet above exemplifies the use of Asynchronous Programming. The method LoadAllVolunteers is marked as async to ensure non-blocking execution, preventing UI freezes during data retrieval.

## Testing

```
[TestFixture]
public class VolunteerTests
{
    private UndacDatabase _testDatabase;
    private string _testDatabasePath;
    private IVolunteerRepository _volunteerRepository;

    [OneTimeSetUp]
    public async Task OneTimeSetUpAsync()
    {
        _testDatabasePath = Path.Combine(TestContext.CurrentContext.TestDirectory, "TestFiles", "TestDB.db3");
        _testDatabase = new UndacDatabase(_testDatabasePath);
        await _testDatabase.Init();
        _volunteerRepository = new VolunteerRepository(_testDatabase.Database);
    }

    [OneTimeTearDown]
    public void TearDownA()
    {
        Directory.Delete(_testDatabasePath);
    }

    [Test]
    public async Task AddVolunteerAsync()
    {
        var volunteerInfo = new Volunteer
        {
            Name = "John Doe",
            Skill = "Medical",
            Status = "Active",
            StatusChangedDate = DateTime.Now,
            ArrivalDate = new DateTime(2023, 12, 09),
            DepartureDate = new DateTime(2024, 03, 09),
            GeographicalLocation = "New York",
            Requested = true
        };

        await _volunteerRepository.SaveAsync(volunteerInfo);

        Volunteer savedVolunteer = await _volunteerRepository.GetAsync(volunteerInfo.Id);

        Assert.That(savedVolunteer, Is.Not.Null, "Volunteer was not saved");
        Assert.That(savedVolunteer.Name, Is.EqualTo(volunteerInfo.Name), "Volunteer name not matching");
    }

    [Test]
    public async Task DeleteVolunteerAsync()
    {
        var volunteerInfo = new Volunteer
        {
            Name = "Jane Smith",
            Skill = "Engineering",
            Status = "Inactive",
            StatusChangedDate = DateTime.Now,
            ArrivalDate = new DateTime(2023, 11, 15),
            DepartureDate = new DateTime(2024, 02, 15),
            GeographicalLocation = "California",
            Requested = true
        };

        await _volunteerRepository.SaveAsync(volunteerInfo);

        await _volunteerRepository.DeleteAsync(await _volunteerRepository.GetAsync(volunteerInfo.Id));

        Volunteer deletedVolunteer = await _volunteerRepository.GetAsync(volunteerInfo.Id);

        Assert.That(deletedVolunteer, Is.Null, "Volunteer was not deleted");
    }

    [Test]
    public async Task GetVolunteersAsync()
    {
        var volunteer1Info = new Volunteer
        {
            Name = "Alice Johnson",
            Skill = "Medical",
            Status = "Active",
            StatusChangedDate = DateTime.Now,
            ArrivalDate = new DateTime(2023, 10, 20),
            DepartureDate = new DateTime(2024, 01, 20),
            GeographicalLocation = "Texas",
            Requested = true
        };

        var volunteer2Info = new Volunteer
        {
            Name = "Bob Brown",
            Skill = "Engineering",
            Status = "Active",
            StatusChangedDate = DateTime.Now,
            ArrivalDate = new DateTime(2023, 09, 10),
            DepartureDate = new DateTime(2024, 02, 10),
            GeographicalLocation = "Arizona",
            Requested = true
        };

        await _volunteerRepository.SaveAsync(volunteer1Info);
        await _volunteerRepository.SaveAsync(volunteer2Info);

        var volunteers = await _volunteerRepository.GetAllAsync();

        var retrievedVolunteer1 = volunteers.FirstOrDefault(volunteer => volunteer.Name == volunteer1Info.Name);
        var retrievedVolunteer2 = volunteers.FirstOrDefault(volunteer => volunteer.Name == volunteer2Info.Name);

        Assert.That(retrievedVolunteer1.Name, Is.EqualTo(volunteer1Info.Name), "First volunteer was not retrieved");
        Assert.That(retrievedVolunteer2.Name, Is.EqualTo(volunteer2Info.Name), "Second volunteer was not retrieved");
```

The test code presented has been written to assess the CRUD (Create, Read, Update, Delete) functionalities concerning volunteers within the system. This test suite is for evaluating the Volunteer module of the system. Its primary objective is to validate operations associated with volunteer data management. The tests have been organized to guarantee the reliability of the system's volunteer-related features. Through systematic testing, the suite aims to ensure the functioning and data integrity within the volunteer management system.

## Reflection

Comments and Documentation: Reviewers emphasized the importance of comments and documentation within the codebase. So I documented the code, including function headers, inline comments. This documentation not only aids in understanding the code logic but also serves as a resource for future developers.

## Issues found when reviewing 

Error Handling and Exception Management: error handling mechanisms were observed, which could result in unhandled exceptions and potential system failures. Insufficient error handling might lead to unexpected crashes and compromise the system's stability and user experience.I suggested implementing robust error handling techniques, such as try-catch blocks, to capture and handle exceptions gracefully. Additionally, providing informative error messages would enhance user experience and simplify troubleshooting.

## Conclusion

During this week's code review, I gained valuable insights into several aspects of software development and team collaboration. I learned the significance of thorough code documentation. Detailed comments not only assist fellow developers but also serve as a future reference point. Effective communication plays a pivotal role in collaborative environments. When project requirements are ambiguous or not clearly defined, team members might interpret tasks differently, leading to inconsistent outcomes.

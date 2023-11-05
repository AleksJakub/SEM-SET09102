## Description
This week, I focused on implementing immediate security alert notifications for end users and efficient communication of security issues to senior management.

## Code Description
```
public EditSecurityAlerts(SecurityAlert securityalert, ISecurityAlertRepository securityalertRepo)
{
    InitializeComponent();
    this.securityalert = securityalert;
    _securityalertsRepo = securityalertRepo;
}
```
**Dependency Injection:**
We're passing in the SecurityAlert object and the ISecurityAlertRepository interface through the constructor. It helps keep things modular and makes testing easier.

**Initialization Separation:**
The constructor only handles setup stuff. The real action, like the logic for resolving or snoozing alerts, happens in other parts of the code. This way, each part of the code has its own job, keeping things clear and simple.

```
if (PickerStatus.SelectedItem.ToString() == "Resolved")
{
    securityalert.Resolved = true;
    securityalert.Snoozed = false;
    await _securityalertsRepo.SaveAsync(securityalert);
    await Navigation.PopAsync();
}
else
{
    securityalert.Snoozed = true;
    securityalert.Resolved = false;
    await _securityalertsRepo.SaveAsync(securityalert);
    await Navigation.PopAsync();
}
```
**Clarity and Readability:**
The code is straightforward and easy to follow. Used clear variable names like securityalert and PickerStatus, which makes it easy to understand. Keeping things simple and easy to read is a big deal in good software design.

**Consistency:**
Kept the code structure the same inside the if and else blocks. This consistency not only makes it easier to read but also helps when someone needs to make changes. Keeping things uniform like this is a smart way to make sure the code stays readable and easy to maintain.

## Testing

```
var testSecurityAlert = new SecurityAlert
{
    Description = "Test",
    Snoozed = false,
    Resolved = false,
    Date = DateTime.Now,
};
await repo.SaveAsync(testSecurityAlert);

// We check if the saved alert matches what we created.
SecurityAlert readSecurityAlert = await repo.GetAsync(testSecurityAlert.Id);
Assert.That(readSecurityAlert, Is.Not.Null, "Test Security Alert was not saved");
Assert.That(readSecurityAlert.Description, Is.EqualTo(testSecurityAlert.Description), "Test Security Alert description not matching");
```
**Testing Adding Security Alerts:**
A test to check if we can add new security alerts properly. We create a test security alert, save it, then retrieve it back from the repository. We're making sure that what we save is what we get.

```
var testSecurityAlert = new SecurityAlert
{
    Description = "Test2",
    Snoozed = false,
    Resolved = true,
    Date = new DateTime(2023, 12, 09),
};
await repo.SaveAsync(testSecurityAlert);

// We delete the test alert and then try to retrieve it.
await repo.DeleteAsync(await repo.GetAsync(testSecurityAlert.Id));
var readSecurityAlert2 = await repo.GetAsync(testSecurityAlert.Id);
Assert.That(readSecurityAlert2, Is.Null, "Test Security Alert was not deleted");
```
**Testing Deleting Security Alerts:**
A test to make sure we can delete security alerts. We create a test alert, save it, delete it, and then try to retrieve it. If we can't retrieve it, we know it was deleted properly.

```
var testSecurityAlert = new SecurityAlert
{
    Description = "Test",
    Snoozed = false,
    Resolved = false,
    Date = DateTime.Now,
};
var testSecurityAlert2 = new SecurityAlert
{
    Description = "Test2",
    Snoozed = false,
    Resolved = true,
    Date = new DateTime(2023, 12, 09),
};
await repo.SaveAsync(testSecurityAlert);
await repo.SaveAsync(testSecurityAlert2);

// We retrieve the saved alerts and check if they match what we created.
var securityalertList = await repo.GetAllAsync();
var securityalert1 = securityalertList.FirstOrDefault(securityalert => securityalert.Description == testSecurityAlert.Description);
var securityalert2 = securityalertList.FirstOrDefault(securityalert => securityalert.Description == testSecurityAlert2.Description);
```
**Testing Retrieving Security Alerts:**
A test to check if we can retrieve security alerts properly. We save two alerts, then try to get them from the repository. We're ensuring that the alerts we save are stored correctly and can be fetched when needed.

## Requested Changes

**Adding Doxygen Comments:**

In response to the code review feedback, I added Doxygen comments to the methods and classes, providing clear documentation for each function's purpose, input parameters, and expected output. These comments serve as a helpful reference for developers working with the codebase, promoting better understanding and collaboration within the team.

**Improving Test Scenarios:**

I enhanced the test cases to cover a broader range of scenarios. This included adding tests for edge cases, invalid inputs, and exceptional situations. By expanding the test, we ensure that our code can handle various scenarios, making the application more robust to unexpected conditions.

**Fixing Filtering Functionality:**

I addressed the issue with the filtering functionality, ensuring that the filtering criteria are applied correctly when retrieving security alerts. By reviewing the filtering logic and making adjustments, I improved the accuracy of the search functionality, allowing users to find specific alerts more effectively based on their descriptions.

## Code Review Issues

No major issues found during the code review process, indicating strong team collaboration and code quality.

Only issue found was a result of not meeting the acceptance criteria noted in the issue, a small overlooking on the developers part and an easy fix

## Reflection

**New Realizations:**
This week, I found out how important it is to write clear and simple explanations in the code. Adding comments that explain what each part does helped my teammates understand the code better. It made working together much easier.

**Common Problems in Team Development:**
One common problem in working with a team is making sure everyone codes in a similar way. We need to talk openly and review each other's code regularly to make sure we all follow the same rules. It's all about making sure everyone understands the best way to write code.

**Personal Improvements:**
This week, I got better at finding and fixing issues in the code. I also got into the habit of documenting my work clearly and testing it thoroughly. Having a step-by-step approach helps me catch mistakes early and make the code stronger. This way, I'm making sure our software is as good as it can be.

In summary, this week showed me the value of clear explanations, consistent coding, and thorough testing. These things not only improved our project but also made me a more effective team player, contributing to a better way of working together.

https://github.com/xinjoonha/SET09102_PURPLE/tree/feature/view-security-alerts/Undac

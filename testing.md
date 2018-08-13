# Testing

## PHPUnit

### General

#### Code

Any repeated code in tests should be pulled into its own method whenever possible.

If it is something that is shared across multiple tests such as a service class
it should be created in the setUp method and assigned to a public variable.

#### Names

Should be clear and concise, and use the test doc block

##### Bad:

```php
function test_new_user()
{
    ...
}
```

##### Good:

```php
/** @test */
function user_can_register()
{
    ...
}
```

#### Negative Tests

When possible and reasonable, separate tests should be created to test
for possible negative effects or invalid input.

For example, a user cannot register using a bad email address, or a
method throws an exception when bad data is passed in.

### Unit Tests

#### Size

Unit test should only test one piece of code and for one result.

##### Bad:

```php
/** @test */
function user_setting_service()
{
    $service = app(UserSettingsService);
    
    $service->updatePhoneNumber([
        'phone' => '555-555-5555',
    ]);
    
    $service->updateEmail([
        'email' => 'test@test.com',
    ]);
    
    $this->assertDatabaseHas('user_settings', [
        'phone' => '555-555-5555',
    ]);
    
    $this->assertDatabaseHas('user_settings', [
        'email' => 'test@test.com',
    ]);
}
```

##### Good:

```php
function setUp()
{
    $this->service = app(UserSettingsService);
}

/** @test */
function user_setting_service_can_update_phone_number()
{   
    $phone = '555-555-5555';
    $this->service->updatePhoneNumber([
        'phone' => $phone,
    ]);
    
    $this->assertDatabaseHas('user_settings', [
        'phone' => $phone,
    ]);
}

/** @test */
function user_setting_service_can_update_email()
{
    $email = 'test@test.com';
    $this->service->updateEmail([
        'email' => $email,
    ]);
    
    $this->assertDatabaseHas('user_settings', [
        'email' => $email,
    ]);
}
```

### Functional Tests

Functional tests are an end to end test.  They can include hitting API/HTTP end points.

For example, new user registration or updating settings.

### Dusk Tests

These tests should be the final tests created.
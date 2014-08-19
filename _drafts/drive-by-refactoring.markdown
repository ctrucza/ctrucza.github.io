---
layout: post
title:  "Drive-by refactoring: Abused Exceptions"
date:   2014-08-19 12:15:41
categories: drive-by refactoring, exceptions
---

Here is a code I was reviewing these days:

{% highlight C# %}
public void SetAccountEnabled(string accountName, bool enabled)
{
    try
    {
        accounts.Single(a => a.Name == accountName).Enabled = enabled;
        SaveChanges();
    }
    catch(InvalidOperationException)
    {
        accounts.Add( 
            new Account { 
                Name = accountName, 
                Enabled = enabled
            });
        SaveChanges();
    }
}
{% endhighlight %}

I have the following issues:

- exception usage: the fact that an `Account` does not exists does not look like an exceptional condition. If it is not `SingleOrDefault` should be used. Even better, `AccountExists(accountName)` should be extracted. 

- naming: the method is called `SetAccountEnabled`, but does something else: sets the enabled flag of the account if it exists, otherwise it creates a new one. It should be `SetAccountEnabledOrCreate`

- naming: the method exposes unneeded details: the fact that there is an `Enabled` flag on the `Account`. There should be two different methods: `EnableAccount` and `DisableAccount`.

First, let's remove the `try-catch`:

{% highlight C# %}
public void SetAccountEnabled(string accountName, bool enabled)
{
    var account = accounts.SingleOrDefault(a => a.Name == accountName);
    if (account == null)
    {
        account = new Account {
                Name = accountName,
                Enabled = enabled
            };
        accounts.Add(account);
    }
    account.Enabled = enabled;
    SaveChanges();
}
{% endhighlight %}

Next extract some meaningful methods:

{% highlight C# %}
private bool AccountExists(string accountName)
{
    return accounts.SingleOrDefault(a => a.Name == accountName) != null;
}

private Account GetAccount(string accountName)
{
    return accounts.Single(a => a.Name == accountName);
}

private Account CreateAccount(string accountName)
{
    var account = new Account {Name = accountName};
    accounts.Add(account);
    return account;
}

public void SetAccountEnabled(string accountName, bool enabled)
{
    Account account = null;
    if (AccountExists(accountName))
    {
        account = GetAccount(accountName);                
    }
    else
    {
        account = CreateAccount(accountName);
    }
    account.Enabled = enabled;
    SaveChanges();
}
{% endhighlight %}

Now this looks pretty verbose. Let's proceed though...

Hide the Enabled flag:

{% highlight C# %}
public void EnableAccount(string accountName)
{
    Account account = null;
    if (AccountExists(accountName))
    {
        account = GetAccount(accountName);                
    }
    else
    {
        account = CreateAccount(accountName);
    }
    account.Enabled = true;
    SaveChanges();
}

public void DisableAccount(string accountName)
{
    Account account = null;
    if (AccountExists(accountName))
    {
        account = GetAccount(accountName);
    }
    else
    {
        account = CreateAccount(accountName);
    }
    account.Enabled = false;
    SaveChanges();
}
{% endhighlight %}

... extract the duplicate code:

{% highlight C# %}
private Account GetOrCreateAccount(string accountName)
{
    Account account = null;
    if (AccountExists(accountName))
    {
        account = GetAccount(accountName);
    }
    else
    {
        account = CreateAccount(accountName);
    }
    return account;
}

public void EnableAccount(string accountName)
{
    var account = GetOrCreateAccount(accountName);
    account.Enabled = true;
    SaveChanges();
}

public void DisableAccount(string accountName)
{
    var account = GetOrCreateAccount(accountName);
    account.Enabled = false;
    SaveChanges();
}
{% endhighlight %}

... inline the originally extracted methods:

{% highlight C# %}
    private Account GetOrCreateAccount(string accountName)
    {
        Account account = accounts.SingleOrDefault(a => a.Name == accountName);
        if ( account == null)
        {
            account = new Account {Name = accountName};
            accounts.Add(account);
        }
        return account;
    }
{% endhighlight %}

... to get to the final result:

{% highlight C# %}
private Account GetOrCreateAccount(string accountName)
{
    Account account = accounts.SingleOrDefault(a => a.Name == accountName);
    if ( account == null)
    {
        account = new Account {Name = accountName};
        accounts.Add(account);
    }
    return account;
}

public void EnableAccount(string accountName)
{
    var account = GetOrCreateAccount(accountName);
    account.Enabled = true;
    SaveChanges();
}

public void DisableAccount(string accountName)
{
    var account = GetOrCreateAccount(accountName);
    account.Enabled = false;
    SaveChanges();
}
{% endhighlight %}

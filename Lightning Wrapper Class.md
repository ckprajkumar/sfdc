### Wrapper Class
The wrapper class instances are created client side, which is easy in JavaScript, mainly due to its loose typing. I can just define an object instance that contains an account and a selected property. In the snippet below
```sh
public class WrapperClass
{
    @AuraEnabled
    public Integer count;
    @AuraEnabled
    public Account acc;
}
```
You can store multiple list in single wrapper class.
#### Apex Class
```sh
public class WClassController{
    @AuraEnabled
    public static List<WrapperClass> getAccounts() {
        List<WrapperClass> wcList = new List<WrapperClass>();
        List<Account> accList = [SELECT Name,Active__c, (SELECT Name FROM contacts) FROM Account];
         
        if(accList.size() > 0){
            for(Account acct : accList){
                if(acct.contacts.size() > 0){
                    WrapperClass wc = new WrapperClass();
                    wc.count = acct.contacts.size();
                    wc.acc = acct;
                    wcList.add(wc);
                }
            }
        }
        return wcList;
    }
}
```
### Lightning Component
```sh 
<aura:component controller="bklightning.WClassController">
    <aura:attribute name="accounts" type="bklightning.WrapperClass[]"/>
    <aura:handler name="init" value="{!this}" action="{!c.doInit}" />
    <div>
        <table class="table table-bordered">
            <thead>
                <tr>
                    <th><b>Name</b></th>
                    <th><b>Contact</b></th>
                    <th><b>Active</b></th>
                </tr>
            </thead>
            <tbody>
                <aura:iteration items="{!v.accounts}" var="account">
                    <tr>
                        <td>{!account.acc.Name}</td>
                        <td>{!account.count}</td>
                        <td>{!account.acc.bklightning__Active__c}</td>
                    </tr>
                </aura:iteration>
            </tbody>
        </table>
    </div>
</aura:component>
```


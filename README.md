# pritesh-shendekar-demo
2)Build a basic lightning component that can query a list of 10 most recently created accounts and display it using a lightning app. 
------------------------------------------------------------------------------------------------
AccountList.cls (Apex Controller):
public class AccountController {
    @AuraEnabled
    public static List<Account> getRecentAccounts() {
        return [SELECT Id, Name FROM Account ORDER BY CreatedDate DESC LIMIT 10];
    }
}

-----------------------------------------------------------------------------------------------------
AccountList.html (html Lightning Component):
<aura:component controller="AccountController">
    <aura:attribute name="accounts" type="Account[]" />
    
    <aura:handler name="init" value="{!this}" action="{!c.doInit}" />
    
    <ul>
        <aura:iteration items="{!v.accounts}" var="acc">
            <li>{!acc.Name}</li>
        </aura:iteration>
    </ul>
    </aura:component>
------------------------------------------------------------------------------------------------------
AccountList.js (js Lightning Component):
<template>
   ({
    doInit: function(component, event, helper) {
        
        var action = component.get("c.getRecentAccounts");
        
        action.setCallback(this, function(response) {
            var state = response.getState();
            if (state === "SUCCESS") {
                component.set("v.accounts", response.getReturnValue());
            }
        })
        $A.enqueueAction(action); 
	}
 })
 
</template>
------------------------------------------------------------------------------------------------------------
AccountList.js-meta-xml (js-meta-xml Lightning Component):
<?xml version="1.0" encoding="UTF-8"?>
<LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata">
    <apiVersion>58.0</apiVersion>
    <isExposed>true</isExposed>
	<targets>
		<target>lightning__RecordPage</target>
		<target>lightning__AppPage</target>
		<target>lightning__HomePage</target>
	</targets>
</LightningComponentBundle>
---------------------------------------------------------------------------------------------------------------
3)Make a basic http callout and print the result using system.debug

public class HttpCalloutExample {
    public void makeHttpCallout() {
        Http http = new Http();
        HttpRequest req = new HttpRequest();
        HttpResponse res = new HttpResponse();
        
        req.setEndpoint('https://postman-echo.com/get?foo1=bar1&foo2=bar2');
        req.setMethod('GET');
        
        try {
            res = http.send(req);
            if (res.getStatusCode() == 200) {
                String responseBody = res.getBody();
                System.debug('HTTP Response: ' + responseBody);
            } else {
                System.debug('HTTP Request failed with status code: ' + res.getStatusCode());
            }
        } catch (Exception e) {
            System.debug('HTTP Request failed with exception: ' + e.getMessage());
        }
    }
}


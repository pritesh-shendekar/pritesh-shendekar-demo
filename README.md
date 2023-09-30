# pritesh-shendekar-demo

-------------------------------------------------------------------------------------------------------
AccountController.cls (Apex Controller):
public class AccountController {
    @AuraEnabled
    public static List<Account> getRecentAccounts() {
        return [SELECT Id, Name FROM Account ORDER BY CreatedDate DESC LIMIT 10];
    }
}
-----------------------------------------------------------------------------------------------------
AccountList.cmp (http Lightning Component):
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

Auther -Pritesh Shendekar

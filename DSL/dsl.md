
AMon Domain-Specific Monitoring Adaptation Language

To facilitate the specification of adaptation rules for a runtime monitoring environment, we created a domain-specific language that provides capabilities for defining different types of adaptation rules and specifying assumptions about the monitoring data.

We include two types of rules:

-. Preplanned Rules that are triggered based on different states of the system, or of its devices (e.g., a UAV being in a "takeoff" state versus "flying")
-. Ubiquitous Rules that are triggered by exceptions (e.g., a low battery warning) or other factors.

In addition to the specification of rules, we also support the description of 
- Assumptions about the system and its environment.
- Default Values



## Preplanned Rules

Preplanned adaptations of the monitoring environment may occur as the CPS  changes state, and these adaptations may require changing the monitoring location from the edge device (e.g., onboard the UAV) to a  centralized monitoring component.
Such rules can either apply _globally_, or to _specific devices_ (e.g., a group of UAVs performing a joint mission, or even individual devices identified by their respective id).
The _Context_ element in the _Trigger_ condition indicates the UAV's current state. 
Subsequently, each  _Monitor_ entry denotes a property item (i.e., a message) that is collected and distributed by the monitoring system. 





<object data="https://github.com/LIT-Rumors/amon-material/blob/main/DSL/RT1.pdf" type="application/pdf" width="700px" height="700px">
    <embed src="https://github.com/LIT-Rumors/amon-material/blob/main/DSL/RT1.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="http://yoursite.com/the.pdf">Download PDF</a>.</p>
    </embed>
</object>



Monitor specifications define the _Scope_ i.e., whether the data is processed locally on the device _scope local_, or sent to a central server (_scope central_) for processing, constraint checking, or further analysis.
The _Period_ further specifies how often data is sent from, for example, the edge device to the central server.


## Ubiquitous Rules


## Default Values

## Assumptions




### Grammar

[XText DSL File](Dsl.xtext)


```Model: 	namespace=Namespace contracts+=Assumption* defaults+=DefaultValues* prules+=MonitoringRule*  grules+=GlobalRule* constraints+=ConstraintRule*;```

``Namespace:	'namespace' namespace=STRING ';';``


``Assumption: 	'Assumption' name=STRING frequency=MinFrequency  ';';``

``DefaultValues:	'Default' name=STRING value=State ';';``

`` State: 	state=(OFF|KEEP);``



``MinFrequency:	'MIN_PERIOD:' frequency=DOUBLE 'seconds';``

``ConstraintRule:	'Constraint' name=STRING scope=Scope  'requiresmonitor' monitors+=STRING* ';';``

``Applicability:	'applies' trigger=(ApplicationID | ApplicationGlobal | ApplicationGroup);``

``ApplicationID:	'for' 'device' '[' event=STRING ']';``

``ApplicationGroup:	'for' 'group' '(' group=STRING ')';``

``ApplicationGlobal:	sc='forall';``

``Scope:	'scope' scope=('local' | 'central');``

``MonitoringRule:	'Rule' 'PREPLANNED' name=STRING applicability=Applicability  trigger=Trigger monitors+=Monitor* ';';``
	
``GlobalRule:	'Rule' 'UBIQUITOUS' name=STRING applicability=Applicability ('salience' salience=INT)?  trigger=Trigger monitors+=Monitor*  ';';``
	

``Trigger:	'Trigger' trigger=(Event | Context | Data);``

``Context:	'Context' '[' context=STRING ']' status?=Status?;``

``Status:	'ENTRY' | 'EXIT';``

``Event:	'Event' '[' event=STRING ']' ('and' 'Event' '[' ontop+=STRING ']')* ;``

``Data:	'Data' '[' value=STRING 'when' condition=ValueCondition ']';``


``Monitor:	'Monitor' name=STRING ':' changes+=ChangeType*;``

``ChangeType:	ChangeFrequency | ChangeState | ChangeScope | ChangeReportingFrequency;``

``ChangeScope:	'CHANGE_SCOPE:' scope=Scope;``

``ChangeState:	'CHANGE_STATE:' scope=(ON | OFF);``

``ChangeFrequency:	'CHANGE_PERIOD:' 'every' frequency=DOUBLE 'seconds';``

``ChangeReportingFrequency:	'CHANGE_REPORTING_PERIOD:' 'every' frequency=DOUBLE 'seconds';``
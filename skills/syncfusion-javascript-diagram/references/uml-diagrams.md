# UML Diagrams

## Table of Contents

- [Overview](#overview)
- [UML Class Diagrams](#uml-class-diagrams)
- [Classifier Shapes](#classifier-shapes)
- [UML Relationships](#uml-relationships)
- [UML Sequence Diagrams](#uml-sequence-diagrams)

## Overview

UML (Unified Modeling Language) diagrams help visualize software architecture and design. Syncfusion Diagram supports UML class diagrams and sequence diagrams for software modeling.

**Key Concepts:**
- **Class Diagrams** - Show system structure, classes, interfaces, relationships
- **Sequence Diagrams** - Show interactions over time between objects
- **Lifelines** - Vertical lines representing object existence
- **Messages** - Interactions between objects

## UML Class Diagrams

### Creating UML Classes

```ts
import { Diagram, NodeModel, UmlClassifierShapeModel } from '@syncfusion/ej2-diagrams';

let umlClass: NodeModel = {
  id: 'employee',
  offsetX: 250,
  offsetY: 250,
  width: 150,
  height: 200,
  
  style: { fill: '#26A0DA' },
  
  shape: {
    type: 'UmlClassifier',
    classifier: 'Class',
    
    // Class structure
    classShape: {
      name: 'Employee',           // Class name
      
      // Class attributes/properties as objects
      attributes: [
        { name: 'id', type: 'int' },
        { name: 'name', type: 'string' },
        { name: 'salary', type: 'decimal' },
        { name: 'department', type: 'string' }
      ],
      
      // Class methods as objects
      methods: [
        { name: 'getDetails', type: 'void' },
        { name: 'updateSalary', type: 'void', parameters: [{ name: 'amount', type: 'decimal' }] },
        { name: 'calculateBonus', type: 'decimal' }
      ]
    }
  } as UmlClassifierShapeModel
};

let diagram = new Diagram({
  nodes: [umlClass]
});

diagram.appendTo('#diagram');
```

## Classifier Shapes

### Interface Classifier

```ts
let interfaceClassifier: NodeModel = {
  id: 'interface',
  offsetX: 100,
  offsetY: 100,
  width: 150,
  height: 150,
  
  style: { fill: '#26A0DA' },
  
  shape: {
    type: 'UmlClassifier',
    classifier: 'Interface',
    
    interfaceShape: {
      name: 'IEmployee',
      
      // Interface methods (all public)
      methods: [
        { name: 'getDetails', type: 'void' },
        { name: 'updateRecord', type: 'void' },
        { name: 'getAddress', type: 'string' }
      ]
    }
  } as UmlClassifierShapeModel
};
```

### Enumeration Classifier

```ts
let enumClassifier: NodeModel = {
  id: 'enum',
  offsetX: 100,
  offsetY: 100,
  width: 150,
  height: 150,
  
  style: { fill: '#26A0DA' },
  
  shape: {
    type: 'UmlClassifier',
    classifier: 'Enumeration',
    
    enumerationShape: {
      name: 'Department',
      
      // Enumeration members
      members: [
        { name: 'HR' },
        { name: 'IT' },
        { name: 'Sales' },
        { name: 'Finance' }
      ]
    }
  } as UmlClassifierShapeModel
};
```

## UML Relationships

### Association Relationship

```ts
import { Diagram, ConnectorModel } from '@syncfusion/ej2-diagrams';

let associationConnector: ConnectorModel = {
  id: 'association',
  sourcePoint: { x: 100, y: 100 },
  targetPoint: { x: 300, y: 300 },
  type: 'Straight',
  
  shape: {
    type: 'UmlClassifier',
    relationship: 'Association',
    associationType: 'Default'     // Default or BiDirectional
  },
  
  annotations: [{
    content: 'works in',
    offset: 0.5
  }]
};
```

### Generalization (Inheritance)

```ts
let inheritanceConnector: ConnectorModel = {
  id: 'inheritance',
  sourcePoint: { x: 100, y: 100 },
  targetPoint: { x: 300, y: 300 },
  type: 'Straight',
  
  shape: {
    type: 'UmlClassifier',
    relationship: 'Inheritance'    // Triangle for generalization
  }
};
```

### Realization (Implementation)

```ts
let realizationConnector: ConnectorModel = {
  id: 'realization',
  sourcePoint: { x: 100, y: 100 },
  targetPoint: { x: 300, y: 300 },
  type: 'Straight',
  
  shape: {
    type: 'UmlClassifier',
    relationship: 'Realization'    // Dashed line for implementation
  }
};
```

### Composition Relationship

```ts
let compositionConnector: ConnectorModel = {
  id: 'composition',
  sourcePoint: { x: 100, y: 100 },
  targetPoint: { x: 300, y: 300 },
  type: 'Straight',
  
  shape: {
    type: 'UmlClassifier',
    relationship: 'Composition'    // Filled diamond at source
  },
  
  annotations: [{ content: '1..n' }]  // Multiplicity
};
```

### Aggregation Relationship

```ts
let aggregationConnector: ConnectorModel = {
  id: 'aggregation',
  sourcePoint: { x: 100, y: 100 },
  targetPoint: { x: 300, y: 300 },
  type: 'Straight',
  
  shape: {
    type: 'UmlClassifier',
    relationship: 'Aggregation'    // Hollow diamond at source
  },
  
  annotations: [{ content: '1..n' }]
};
```

### Dependency Relationship

```ts
let dependencyConnector: ConnectorModel = {
  id: 'dependency',
  sourcePoint: { x: 100, y: 100 },
  targetPoint: { x: 300, y: 300 },
  type: 'Straight',
  
  shape: {
    type: 'UmlClassifier',
    relationship: 'Dependency'     // Dashed line with arrow
  }
};
```

### Multiplicity Relationship

Multiplicity defines the cardinality (number of instances) of elements in a relationship. Types supported:

```ts
let multiplicityConnector: ConnectorModel = {
  id: 'connector1',
  sourcePoint: { x: 100, y: 100 },
  targetPoint: { x: 300, y: 300 },
  type: 'Straight',
  
  shape: {
    type: 'UmlClassifier',
    relationship: 'Dependency',
    multiplicity: {
      type: 'OneToOne',           // OneToOne, ManyToOne, OneToMany, ManyToMany
      
      // Source cardinality label
      source: {
        optional: true,
        lowerBounds: '1',
        upperBounds: '1'
      },
      
      // Target cardinality label
      target: {
        optional: false,
        lowerBounds: '1',
        upperBounds: '*'            // '*' means unlimited
      }
    }
  }
};
```

## UML Sequence Diagrams

A UML sequence diagram is an interaction diagram that demonstrates how objects interact with each other and the order of these interactions. Sequence diagrams are created using the `UmlSequenceDiagramModel` and assigned to the `model` property.

### UML Sequence Diagram Setup

```ts
import { Diagram, UmlSequenceDiagramModel, UmlSequenceMessageType, SnapConstraints } from '@syncfusion/ej2-diagrams';

// Define the model for the UML Sequence Diagram
const umlSequenceDiagramModel: UmlSequenceDiagramModel = {
  // Define participants in the sequence diagram
  participants: [
    { id: 'client', content: 'Client', isActor: true },
    { id: 'server', content: 'Server', isActor: false },
    { id: 'database', content: 'Database', isActor: false }
  ],
  
  // Define messages exchanged between participants
  messages: [
    { id: 'MSG1', content: 'Request', fromParticipantID: 'client', toParticipantID: 'server', type: UmlSequenceMessageType.Synchronous },
    { id: 'MSG2', content: 'Query', fromParticipantID: 'server', toParticipantID: 'database', type: UmlSequenceMessageType.Synchronous },
    { id: 'MSG3', content: 'Data', fromParticipantID: 'database', toParticipantID: 'server', type: UmlSequenceMessageType.Reply },
    { id: 'MSG4', content: 'Response', fromParticipantID: 'server', toParticipantID: 'client', type: UmlSequenceMessageType.Reply }
  ]
};

let diagram = new Diagram({
  width: '100%',
  height: '600px',
  model: umlSequenceDiagramModel,
  snapSettings: { constraints: SnapConstraints.None }
});

diagram.appendTo('#diagram');
```

### Participants

Participants represent the entities (actors or objects) that interact in the sequence diagram. Each participant is displayed with a lifeline extending downward.

```ts
const participants = [
  { 
    id: 'User', 
    content: 'User',                    // Display label
    isActor: true,                      // True for actor, false for object
    showDestructionMarker: false        // Show X marker at end of lifeline
  },
  { 
    id: 'System', 
    content: 'System', 
    isActor: false, 
    showDestructionMarker: true 
  }
];
```

### Messages

Messages represent communication between participants. EJ2 supports multiple message types:

```ts
const messages = [
  // Synchronous message (solid arrow) - sender waits for response
  {
    id: 'MSG1',
    content: 'Request',
    fromParticipantID: 'client',
    toParticipantID: 'server',
    type: UmlSequenceMessageType.Synchronous
  },
  
  // Asynchronous message (open arrow) - sender continues without waiting
  {
    id: 'MSG2',
    content: 'Notify',
    fromParticipantID: 'server',
    toParticipantID: 'client',
    type: UmlSequenceMessageType.Asynchronous
  },
  
  // Reply message (dashed line) - response to previous message
  {
    id: 'MSG3',
    content: 'Response',
    fromParticipantID: 'server',
    toParticipantID: 'client',
    type: UmlSequenceMessageType.Reply
  },
  
  // Self message - participant to itself
  {
    id: 'MSG4',
    content: 'Process',
    fromParticipantID: 'server',
    toParticipantID: 'server',
    type: UmlSequenceMessageType.Self
  },
  
  // Create message - instantiates a new participant
  {
    id: 'MSG5',
    content: 'Create Session',
    fromParticipantID: 'client',
    toParticipantID: 'session',
    type: UmlSequenceMessageType.Create
  },
  
  // Delete message - terminates a participant
  {
    id: 'MSG6',
    content: 'Destroy',
    fromParticipantID: 'session',
    toParticipantID: 'session',
    type: UmlSequenceMessageType.Delete
  }
];
```

### Activation Boxes

Activation boxes (or activation lifelines) represent periods when a participant is active and processing a message.

```ts
const model: UmlSequenceDiagramModel = {
  participants: [
    {
      id: 'User',
      content: 'User',
      isActor: true,
      // Define activation boxes for this participant
      activationBoxes: [
        {
          id: 'ActUser',
          startMessageID: 'MSG1',    // Activation starts at this message
          endMessageID: 'MSG4'       // Activation ends at this message
        }
      ]
    },
    {
      id: 'System',
      content: 'System',
      isActor: false
    }
  ],
  messages: [
    { id: 'MSG1', content: 'Request', fromParticipantID: 'User', toParticipantID: 'System', type: UmlSequenceMessageType.Synchronous },
    { id: 'MSG4', content: 'Response', fromParticipantID: 'System', toParticipantID: 'User', type: UmlSequenceMessageType.Reply }
  ]
};
```

### Fragments (Loops & Conditions)

Fragments group messages based on conditions or loops (Optional, Alternative, Loop).

```ts
import { Diagram, UmlSequenceDiagramModel, UmlSequenceMessageType, UmlSequenceFragmentType } from '@syncfusion/ej2-diagrams';

const model: UmlSequenceDiagramModel = {
  spaceBetweenParticipants: 300,
  
  participants: [
    { id: 'Customer', content: 'Customer', isActor: true },
    { id: 'OrderSystem', content: 'Order System', isActor: false }
  ],
  
  messages: [
    { id: 'MSG1', content: 'Place Order', fromParticipantID: 'Customer', toParticipantID: 'OrderSystem', type: UmlSequenceMessageType.Synchronous },
    { id: 'MSG2', content: 'Check Stock', fromParticipantID: 'OrderSystem', toParticipantID: 'OrderSystem', type: UmlSequenceMessageType.Self },
    { id: 'MSG3', content: 'Confirm', fromParticipantID: 'OrderSystem', toParticipantID: 'Customer', type: UmlSequenceMessageType.Reply }
  ],
  
  // Define fragments for the sequence
  fragments: [
    {
      id: 'frag1',
      type: UmlSequenceFragmentType.Optional,
      conditions: [
        {
          content: 'if item in stock',
          messageIds: ['MSG2', 'MSG3']
        }
      ]
    }
  ]
};
```

## Complete Example: User Login Sequence

A complete user login flow demonstrating interactions between User, Client, Server, and Database:

```ts
import { Diagram, UmlSequenceDiagramModel, UmlSequenceMessageType, UmlSequenceFragmentType, SnapConstraints } from '@syncfusion/ej2-diagrams';

const loginSequenceModel: UmlSequenceDiagramModel = {
  spaceBetweenParticipants: 250,
  
  // Define all participants in the login flow
  participants: [
    { id: 'User', content: 'User', isActor: true },
    { id: 'Client', content: 'Client Application', isActor: false },
    { id: 'Server', content: 'Authentication Server', isActor: false, activationBoxes: [] },
    { id: 'Database', content: 'Database', isActor: false }
  ],
  
  // Define all messages exchanged during login
  messages: [
    // User enters credentials and submits
    { id: 'MSG1', content: 'Enter Credentials', fromParticipantID: 'User', toParticipantID: 'Client', type: UmlSequenceMessageType.Synchronous },
    
    // Client sends login request to server
    { id: 'MSG2', content: 'POST /login', fromParticipantID: 'Client', toParticipantID: 'Server', type: UmlSequenceMessageType.Synchronous },
    
    // Server validates credentials with database
    { id: 'MSG3', content: 'Query User', fromParticipantID: 'Server', toParticipantID: 'Database', type: UmlSequenceMessageType.Synchronous },
    
    // Database returns user data
    { id: 'MSG4', content: 'User Data', fromParticipantID: 'Database', toParticipantID: 'Server', type: UmlSequenceMessageType.Reply },
    
    // Server processes login (internal operation)
    { id: 'MSG5', content: 'Validate & Create Session', fromParticipantID: 'Server', toParticipantID: 'Server', type: UmlSequenceMessageType.Self },
    
    // Server returns session token to client
    { id: 'MSG6', content: 'Session Token', fromParticipantID: 'Server', toParticipantID: 'Client', type: UmlSequenceMessageType.Reply },
    
    // Client displays success and stores token
    { id: 'MSG7', content: 'Login Success', fromParticipantID: 'Client', toParticipantID: 'User', type: UmlSequenceMessageType.Reply }
  ],
  
  // Define activation boxes for participants
  // Server is active during login processing
  activationBoxes: [],
  
  // Define conditional fragments for the flow
  fragments: [
    // Optional: Show only if credentials are valid
    {
      id: 'validationFrag',
      type: UmlSequenceFragmentType.Optional,
      conditions: [
        {
          content: 'if credentials valid',
          messageIds: ['MSG4', 'MSG5', 'MSG6']
        }
      ]
    },
    // Alternative: Success or Failure
    {
      id: 'resultFrag',
      type: UmlSequenceFragmentType.Alternative,
      conditions: [
        {
          content: 'if login successful',
          messageIds: ['MSG6', 'MSG7']
        },
        {
          content: 'else login failed',
          messageIds: ['MSG1']  // User tries again
        }
      ]
    }
  ]
};

// Initialize diagram with sequence model
let diagram = new Diagram({
  width: '100%',
  height: '700px',
  model: loginSequenceModel,
  snapSettings: { constraints: SnapConstraints.None }
});

diagram.appendTo('#sequence-diagram');
```

## Next Steps

- **Swimlanes:** [swimlanes.md](swimlanes.md)
- **BPMN Diagrams:** [bpmn-diagrams.md](bpmn-diagrams.md)
- **Export Diagrams:** [serialization-and-export.md](serialization-and-export.md)

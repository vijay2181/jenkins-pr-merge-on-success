node {
 properties([
  pipelineTriggers([
   [$class: 'GenericTrigger',
    genericVariables: [
     [key: 'number', value: '$.number']//,
    //  [
    //   key: 'before',
    //   value: '$.before',
    //   expressionType: 'JSONPath', //Optional, defaults to JSONPath
    //   regexpFilter: '', //Optional, defaults to empty string
    //   defaultValue: '' //Optional, defaults to empty string
    //  ]
    ],
    // genericRequestVariables: [
    //  [key: 'requestWithNumber', regexpFilter: '[^0-9]'],
    //  [key: 'requestWithString', regexpFilter: '']
    // ],
    // genericHeaderVariables: [
    //  [key: 'headerWithNumber', regexpFilter: '[^0-9]'],
    //  [key: 'headerWithString', regexpFilter: '']
    // ],

    causeString: 'Triggered on $number',

    token: 'abc123',
    tokenCredentialId: '',

    printContributedVariables: true,
    printPostContent: true,

    silentResponse: false,

    regexpFilterText: '$number',
    regexpFilterExpression: env.CHANGE_ID
   ]
  ])
 ])

 stage("build") {
  sh '''
  echo Variables from shell:
  echo number $number
  '''
 }
}

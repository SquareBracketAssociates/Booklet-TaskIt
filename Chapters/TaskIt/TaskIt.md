# TaskIt
  baseline: 'TaskIt';
  repository: 'github://sbragagnolo/taskit';
  load.
  baseline: 'TaskIt';
  repository: 'github://sbragagnolo/taskit:v0.2';
  load.
  baseline: 'TaskIt';
  repository: 'github://sbragagnolo/taskit:dev-0.3';
  load.
    baseline: 'TaskIt'
    with: [ spec repository: 'github://sbragagnolo/taskit-2:v0.2' ]
'Waited' logCr ] schedule.
TKTTask valuable: (MessageSend receiver: 1 selector: #+ arguments: { 7 }).
	instanceVariableNames: ''
	classVariableNames: ''
	package: 'MyPackage'.

MyTask >> value
    ^ 100 factorial
aFuture onSuccessDo: [ :result | result logCr ].
aFuture onFailureDo: [ :error | error sender method selector logCr ].
future onSuccessDo: [ :v | FileStream stdout nextPutAll: v asString; cr ].
future onSuccessDo: [ :v | 'Finished' logCr ].
future onSuccessDo: [ :v | [ v factorial logCr ] schedule ].
future onFailureDo: [ :error | error logCr ].
future onSuccessDo: [ :v | v logCr ].

2 seconds wait.
future onSuccessDo: [ :v | v logCr ].
aRunner schedule: [ 1 second wait. 'test' logCr ].
task := [ 10 timesRepeat: [ 10 milliSeconds wait.
				('Hello from: ', Processor activeProcess identityHash asString) logCr ] ].
aRunner schedule: task.
aRunner schedule: task.
future := aRunner schedule: [ 1 second wait ].
worker start.
worker schedule: [ 1 + 5 ].
worker stop.
future1 := worker future: [ 2 + 2 ].
future2 := worker future: [ 3 + 3 ].
future3 := worker future: [ 1 + 1 ].
pool poolMaxSize: 5.
pool poolMaxSize: 5.
pool start.
pool schedule: [ 1 logCr ].
pool poolMaxSize: 5.
pool poolMaxSize: 5.
pool start.
pool schedule: [ 1 logCr ].
	anError debug
(future collect: [ :number | number factorial ])
    onSuccessDo: [ :result | result logCr ].
(future select: [ :number | number even ])
    onSuccessDo: [ :result | result logCr ];
    onFailureDo: [ :error | error logCr ].
(future flatCollect: [ :number | [ number factorial ] future ])
    onSuccessDo: [ :result | result logCr ].
future2 := [ 18 factorial ] future.
(future1 zip: future2)
    onSuccessDo: [ :result | result logCr ].
    on: Error do: [ :error | 5 ].
future onSuccessDo: [ :result | result logCr ].
successFuture := [ 1 + 1 ] future.
(failFuture fallbackTo: successFuture)
    onSuccessDo: [ :result | result logCr ].
successFuture := [ 1 second wait. 1 + 1 ] future.
(failFuture firstCompleteOf: successFuture)
    onSuccessDo: [ :result | result logCr ];
    onFailureDo: [ :error | error logCr ].
    andThen: [ :result | result logCr ])
    andThen: [ :result | FileStream stdout nextPutAll: result ]. 
[future isFinished] whileFalse: [50 milliseconds wait].
future waitForTimeout: 2 seconds.

future := [1 second wait] future.
[future waitForTimeout: 50 milliSeconds] on: TKTTimeoutException do: [ :error | error logCr ].
(future synchronizeTimeout: 2 seconds) logCr.

future := [ self error ] future.
[ future synchronizeTimeout: 2 seconds ] on: Error do: [ :error | error logCr ].

future := [ 5 seconds wait ] future.
[ future synchronizeTimeout: 1 seconds ] on: TKTTimeoutException do: [ :error | error logCr ].
  instanceVariableNames: 'file'
  classVariableNames: ''
  package: 'TaskItServices-Tests'
  super setUp.
  Transcript show: 'File watcher started'.

TKTFileWatcher >> tearDown
  super tearDown.
  Transcript show: 'File watcher finished'.
  1 second wait.
  file asFileReference exists
    ifFalse: [ Transcript show: 'file does not exist!' ]
  ^ 'Watcher file: ', file asString
watcher file: 'temp.txt'.
watcher start.
service name: 'Generic watcher service'.
service onSetUpDo: [ Transcript show: 'File watcher started' ].
service onTearDownDo: [ Transcript show: 'File watcher finished' ].
service step: [
  'temp.txt' asFileReference exists
    ifFalse: [ Transcript show: 'file does not exist!' ] ].

service start.
  baseline: 'TaskIt';
  repository: 'github://sbragagnolo/taskit';
  load: 'debug'.
|     frame 1     |
-------------------
|     frame 2     |
-------------------
-------------------
|     frame 3     |
-------------------
|     frame 4     |
-------------------
	^ {(#profile -> #development).
	(#development
		-> [ {(#initialize
				-> [ TKTDebugger enable.
					self class runner start ]).
			(#runner -> TKTCommonQueueWorkerPool createDefault).
			(#poolWorkerProcess -> TKTDebuggWorkerProcess).
			(#process -> TKTRawProcess).
			(#errorHandler -> TKTDebuggerExceptionHandler).
			(#processProvider -> TKTTaskItProcessProvider new).
			(#serviceManager -> TKTServiceManager new)} asDictionary ]).
	(#production
		-> [ {(#initialize
				-> [ TKTDebugger disable.
					self class runner start ]).
			(#runner -> TKTCommonQueueWorkerPool createDefault).
			(#poolWorkerProcess -> TKTWorkerProcess).
			(#process -> TKTRawProcess).
			(#errorHandler -> TKTExceptionHandler).
			(#processProvider -> TKTPharoProcessProvider new).
			(#serviceManager -> TKTServiceManager new)} asDictionary ]).
	(#test
		-> [ {(#initialize
				-> [ TKTDebugger disable.
					self class runner start ]).
			(#runner -> TKTCommonQueueWorkerPool createDefault).
			(#poolWorkerProcess -> TKTWorkerProcess).
			(#process -> TKTRawProcess).
			(#errorHandler -> TKTExceptionHandler).
			(#processProvider -> TKTTaskItProcessProvider new).
			(#serviceManager -> TKTServiceManager new)} asDictionary ])} asDictionary.
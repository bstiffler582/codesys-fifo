// Type agnostic / generic type FIFO queue and ring buffer implementations
// Programmed in Visual Studio 2015 using Beckhoff TwinCAT development environment

////////////////////// Usage Sample ///////////////////

// DUTs
TYPE ST_Test :
STRUCT
	iTest : INT;
	fTest : REAL;
END_STRUCT
END_TYPE


// MAIN
// Header / declarations
PROGRAM MAIN
VAR
	// FIFO queue declaration
	fbFifoQueue : FB_FifoQueue;
	
	// test bit toggles
	enqueue : BOOL;
	dequeue : BOOL;
	
	// queue data array
	test_queue : ARRAY[0..4] OF ST_Test;
	
	// item loaded to queue
	item_enqueue : ST_Test;
	
	// item retrieved from queue
	item_dequeue : ST_Test;
	
END_VAR

// BODY

// instantiate FIFO queue
fbFifoQueue(ptrArrData:= ADR(test_queue), Length:=5, Ring:=FALSE);

IF enqueue THEN
	// load struct with dummy data
	item_enqueue.iTest := item_enqueue.iTest + 1;
	item_enqueue.fTest := item_enqueue.fTest + 0.1;
	
	// enqueue item
	fbFifoQueue.Enqueue(item_enqueue);
	
	enqueue := FALSE;
END_IF

IF dequeue THEN
	// dequeue item
	MEMCPY(ADR(item_dequeue), fbFifoQueue.Dequeue(), SIZEOF(item_dequeue));
	
	dequeue := FALSE;
END_IF
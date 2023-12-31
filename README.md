
# FramesDepot Class

## Overview
The `FramesDepot` class is designed to store and manage video frames in shared memory, allowing for controlled access and efficient frame management in asynchronous environments. It's particularly useful in scenarios where frames need to be shared between different processes or handled at different rates.

## Features
- **Fixed-size Queue**: Holds a specified maximum number of frames.
- **Shared Memory**: Utilizes shared memory for efficient inter-process communication.
- **Backpressure Management**: Provides a callback mechanism to handle situations where the frame queue is full.
- **Asynchronous Support**: Designed to work in an asynchronous environment, ensuring safe and efficient operations.

## Usage

### Initialization
```python
frame_size = (width, height, channels)  # Dimensions of the frames
memory_name = "frame_depot"  # Name for the shared memory block
max_size = 10  # Maximum number of frames in the queue

frames_depot = FramesDepot(frame_size, memory_name, max_size)
```

### Adding Frames
```python
# Assume frame is a NumPy array representing a video frame and current_frame is an integer frame number
frame_dict = {"frame_number": current_frame, "frame": frame}
await frames_depot.enqueue(frame_dict)
```

### Retrieving Frames
```python
frame_number = 5  # The specific frame number you want to retrieve
frames_depot.get_frame(frame_number)
```

### Handling Full Queue
Implement a backpressure_callback function that will be called when the queue is full.
```python
def backpressure_callback():
    print("The queue is full. Consider slowing down frame production or handling the surplus.")

frames_depot = FramesDepot(frame_size, memory_name, max_size, backpressure_callback)
```

### Clearing the Queue
```python
await frames_depot.clear_queue()
```

### Closing and Releasing Resources
```python
await frames_depot.close()
```

## Methods

- `__init__(frame_size, memory_name, max_size, backpressure_callback)`: Initializes the FramesDepot.
- `enqueue(frame_dict)`: Asynchronously adds a frame to the queue and shared memory.
- `get_frame(frame_number)`: Retrieves a specific frame by number.
- `clear_queue()`: Asynchronously clears the frame queue and index.
- `close()`: Asynchronously closes and releases all resources, including shared memory.

## Properties

- `queue_is_full`: Indicates whether the queue is full.

## Requirements
- Python 3.7+
- NumPy
- asyncio

## Note
Ensure proper synchronization and error handling in your production environment, especially when dealing with shared resources and asynchronous operations.

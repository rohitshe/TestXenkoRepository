# Pointers

# Definitions

## Pointers

Pointers are points on the device screen corresponding to **finger touches**.

On devices supporting multi-touches,there can be several pointers simultaneously at a given same time.

For the desktop platform pointers are simulated by the mouse left-button. 

## Pointer Events

A @'SiliconStudio.Xenko.Input.PointerEvent' contains **information about a pointer** status. It is thrown every time that a modification happens on the a pointer.

## Pointer States

The @'SiliconStudio.Xenko.Input.PointerState' is an enumeration describing the **action performed by the pointer**. The three principal states are: *Down, Move, Up*.

# Overview

Pointers information is conveyed via @'SiliconStudio.Xenko.Input.PointerEvent'.

Every time that the user touches the screen the detailed sequence of pointer events is buffered. Then it is provided to the programmer on the next frame turn. All pointer events not analysed during a frame turn are lost.

The pointer that triggered a pointer event can be identified using the @'SiliconStudio.Xenko.Input.PointerEvent.PointerId' field.

The **action** that the pointer performed can be found by analyzing the @'SiliconStudio.Xenko.Input.PointerEvent.State' field.

The new **position** of the pointer can be found by analyzing the @'SiliconStudio.Xenko.Input.PointerEvent.Position' field.

# Usage

The @'SiliconStudio.Xenko.Input.IInputManager' interface provides access to the list of @'SiliconStudio.Xenko.Input.PointerEvent' that happened during the last draw call via the **@'SiliconStudio.Xenko.Input.IInputManager.PointerEvents' field**. The list is cleared every frame, so the user does not need to clear it manually. 

The @'SiliconStudio.Xenko.Input.IInputManager' interface is accessible from the @'SiliconStudio.Xenko.Game' and @'SiliconStudio.Xenko.Script' classes via their *Input* property.

## Sample

Here is a simple sample program that tracks of the pointer currently on the screen and print their positions. To have access to the *Input* field, the class inherit from the @'SiliconStudio.Xenko.Script' class.

**Code:** Pointer events sample

```cs
        public override async Task Execute()
        {
            var pointerPositions = new Dictionary<int, Vector2>(); 
            while (true)
            {
                await Scheduler.NextFrame();
                foreach (var pointerEvent in Input.PointerEvents)
                {
                    switch (pointerEvent.State)
                    {
                        case PointerState.Down:
                            pointerPositions[pointerEvent.PointerId] = pointerEvent.Position;
                            break;
                        case PointerState.Move:
                            pointerPositions[pointerEvent.PointerId] = pointerEvent.Position;
                            break;
                        case PointerState.Up:
                        case PointerState.Out:
                        case PointerState.Cancel:
                            pointerPositions.Remove(pointerEvent.PointerId);
                            break;
                        default:
                            throw new ArgumentOutOfRangeException();
                    }
                }
                var positionsStr = pointerPositions.Values.Aggregate("", (current, pointer) => current + (pointer.ToString() + ", "));
                logger.Info("There are currently {0} pointers on the screen located at {1}", pointerPositions.Count, positionsStr);
            }
        }```


# Remarks

- A **pointer event** contains information on **only one pointer**. If several pointers are modified simultaneously one pointer event is sent for each of them.
- Pointer events are **listed by** **chronological order** (time of the event).
- A series of pointer event for a given pointer always **starts by a Down action** then followed by 0 or more **Move** **actions** and ends by an **Up, Out or Cancel action**.
- Pointer **positions are normalized**. (0,0) represents the left-top corner of the screen and (1,1) represents the right-bottom corner of the screen.
- The association *finger <-> pointer ID* is valid only during an *Down->Move->Up* sequence of pointer events. So **a given finger can have different IDs** each time it leaves the screen.
- Pointer events' delta-values (e.g. @'SiliconStudio.Xenko.Input.PointerEvent.DeltaTime' and @'SiliconStudio.Xenko.Input.PointerEvent.DeltaPosition') represent the changes since the last event of the same pointer (same pointer ID). Delta values are always nulls at the beginning a given pointer series of event (e.g. when the pointer state is *Down).*


# Asynchronous Programming

!!! info
    This guide is written from the video [How To Easily Do Asynchronous Programming With Asyncio In Python](https://www.youtube.com/watch?v=2IW-ZEui4h4).

    The code for which can be found [here](https://github.com/ArjanCodes/2021-asyncio/tree/main/after-2).

This example shows you how to use the `asyncio` module in python to asynchronously run code (run in parallel) on your computer. To help guide us through this, we will use an example of controlling three IOT devices, a smart light bulb, a smart speaker and a smart toilet.

=== "`main.py`"
    ``` python
    import asyncio
    from typing import Any, Awaitable

    from iot.devices import HueLightDevice, SmartSpeakerDevice, SmartToiletDevice
    from iot.message import Message, MessageType
    from iot.service import IOTService


    async def run_sequence(*functions: Awaitable[Any]) -> None:
        for function in functions:
            await function


    async def run_parallel(*functions: Awaitable[Any]) -> None:
        await asyncio.gather(*functions)


    async def main() -> None:
        # create a IOT service
        service = IOTService()

        # create and register a few devices
        hue_light = HueLightDevice()
        speaker = SmartSpeakerDevice()
        toilet = SmartToiletDevice()

        hue_light_id, speaker_id, toilet_id = await asyncio.gather(
            service.register_device(hue_light),
            service.register_device(speaker),
            service.register_device(toilet),
        )

        # create a few programs
        wake_up_program = [
            Message(hue_light_id, MessageType.SWITCH_ON),
            Message(speaker_id, MessageType.SWITCH_ON),
            Message(speaker_id, MessageType.PLAY_SONG, "Miles Davis - Kind of Blue"),
        ]

        # run the programs
        await service.run_program(wake_up_program)
        await run_parallel(
            service.send_msg(Message(hue_light_id, MessageType.SWITCH_OFF)),
            service.send_msg(Message(speaker_id, MessageType.SWITCH_OFF)),
            run_sequence(
                service.send_msg(Message(toilet_id, MessageType.FLUSH)),
                service.send_msg(Message(toilet_id, MessageType.CLEAN)),
            ),
        )


    if __name__ == "__main__":
        asyncio.run(main())
    ```

=== "`iot/devices.py`"
    ``` python
    import asyncio

    from iot.message import MessageType


    class HueLightDevice:
        async def connect(self) -> None:
            print("Connecting Hue Light.")
            await asyncio.sleep(0.5)
            print("Hue Light connected.")

        async def disconnect(self) -> None:
            print("Disconnecting Hue Light.")
            await asyncio.sleep(0.5)
            print("Hue Light disconnected.")

        async def send_message(self, message_type: MessageType, data: str = "") -> None:
            print(
                f"Hue Light handling message of type {message_type.name} with data [{data}]."
            )
            await asyncio.sleep(0.5)
            print("Hue Light received message.")


    class SmartSpeakerDevice:
        async def connect(self) -> None:
            print("Connecting to Smart Speaker.")
            await asyncio.sleep(0.5)
            print("Smart Speaker connected.")

        async def disconnect(self) -> None:
            print("Disconnecting Smart Speaker.")
            await asyncio.sleep(0.5)
            print("Smart Speaker disconnected.")

        async def send_message(self, message_type: MessageType, data: str = "") -> None:
            print(
                f"Smart Speaker handling message of type {message_type.name} with data [{data}]."
            )
            await asyncio.sleep(0.5)
            print("Smart Speaker received message.")


    class SmartToiletDevice:
        async def connect(self) -> None:
            print("Connecting to Smart Toilet.")
            await asyncio.sleep(0.5)
            print("Smart Toilet connected.")

        async def disconnect(self) -> None:
            print("Disconnecting Smart Toilet.")
            await asyncio.sleep(0.5)
            print("Smart Toilet disconnected.")

        async def send_message(self, message_type: MessageType, data: str = "") -> None:
            print(
                f"Smart Toilet handling message of type {message_type.name} with data [{data}]."
            )
            await asyncio.sleep(0.5)
            print("Smart Toilet received message.")
    ```

=== "`iot/service.py`"
    ``` python
    import asyncio
    import random
    import string
    from typing import Any, Awaitable, Protocol

    from iot.message import Message, MessageType


    def generate_id(length: int = 8):
        return "".join(random.choices(string.ascii_uppercase, k=length))


    class Device(Protocol):
        async def connect(self) -> None:
            ...

        async def disconnect(self) -> None:
            ...

        async def send_message(self, message_type: MessageType, data: str = "") -> None:
            ...


    class IOTService:
        def __init__(self):
            self.devices: dict[str, Device] = {}

        async def register_device(self, device: Device) -> str:
            await device.connect()
            device_id = generate_id()
            self.devices[device_id] = device
            return device_id

        async def unregister_device(self, device_id: str) -> None:
            await self.devices[device_id].disconnect()
            del self.devices[device_id]

        def get_device(self, device_id: str) -> Device:
            return self.devices[device_id]

        async def run_program(self, program: list[Message]) -> None:
            print("=====RUNNING PROGRAM======")
            await asyncio.gather(*[self.send_msg(msg) for msg in program])
            print("=====END OF PROGRAM======")

        async def send_msg(self, msg: Message) -> None:
            await self.devices[msg.device_id].send_message(msg.msg_type, msg.data)
    ```

=== "`iot/message.py`"
    ``` python
    from dataclasses import dataclass
    from enum import Enum, auto


    class MessageType(Enum):
        SWITCH_ON = auto()
        SWITCH_OFF = auto()
        CHANGE_COLOR = auto()
        PLAY_SONG = auto()
        OPEN = auto()
        CLOSE = auto()
        FLUSH = auto()
        CLEAN = auto()


    @dataclass
    class Message:
        device_id: str
        msg_type: MessageType
        data: str = ""
    ```
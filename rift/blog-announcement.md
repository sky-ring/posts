# Unlocking the Power of TON with Rift Framework
Decentralized technology is the future. With its ability to bring about true democratization and fairness, the world is gravitating towards blockchain and its many applications. One such blockchain is The Open Network (TON), an ultra-fast, low-fee, and environmentally friendly layer-1 blockchain designed to onboard billions of users.
<!-- 
It's time to take your **TON development** to the next level with **Rift**, the full-stack development framework that makes it easier than ever to build on TON. -->

## The Power of Python and TON ⚡️
TON is a decentralized layer-1 blockchain designed for fast and efficient transactions. But despite its many benefits, TON's ***FunC*** and ***Fift*** programming languages can be complex for developers to learn. That's where Rift comes in.

With Rift, developers can leverage the simplicity and versatility of Python to build and interact with TON. Python's syntax and object-oriented programming (OOP) features make it easy for developers to write and test smart contracts, without having to learn a new programming language.

Rift is the magical portal that links **Python** to **TON** world.

![Rift Portal](./rift-portal.png)

## Simplifying the Development Process 💻
Rift offers a full suite of tools for TON development. From writing smart contracts in Python to testing and deploying them on the TON network, Rift streamlines every step of the development process. And with its standalone framework, developers only need to know Python 3.10 to start building on TON.

Here are just a few of the many features that make Rift an indispensable tool for TON development:

### Power up your Smart Contract Development with Rift 🔥
With Rift, developers can take full advantage of Python’s syntax and capabilities to develop TON smart contracts with ease. No need to learn a new programming language or worry about compatibility issues, Rift brings the best of both worlds and makes TON development accessible to all.

```python
class SimpleWallet(Contract):
    """
    Simple Wallet Contract.

    # config
    get-methods:
        - seq_no
        - public_key
    """

    class Data(Model):
        seq_no: uint32
        public_key: uint256

    class ExternalBody(SignedPayload):
        seq_no: uint32
        valid_until: uint32

    data: Data

    def external_receive(self) -> None:
        msg = self.body % self.ExternalBody
        assert msg.valid_until > std.now(), 35
        assert msg.seq_no == self.data.seq_no, 33
        assert msg.verify_signature(self.data.public_key), 34
        std.accept_message()
        while msg.refs():
            mode = msg >> uint8
            std.send_raw_message(msg >> Ref[Cell], mode)
        self.data.seq_no += 1
        self.data.save()
```

### Interact with TON like a Pro 💫
Rift provides an all-in-one wrapper around TON development tools, making it easier than ever to interact with the TON network. With Rift, developers can build messages, sign them using the Fift backend, run contract codes, deploy contracts, and interact with the network using the Tonlibjson backend.

```python
from contracts.jetton_minter import JettonMinter
from contracts.jetton_wallet import JettonWallet


def deploy():
    init_data = JettonMinter.Data()
    init_data.admin = "EQCDRmpCsiy5fA0E1voWMpP-L4SQ2lX0liTk3zgFXcyLSYS3"
    init_data.total_supply = 10**11  # 100
    init_data.content = Cell()
    init_data.wallet_code = JettonWallet.code()
    msg, addr = JettonMinter.deploy(init_data, amount=2 * 10 ** 8)
    return msg, False
```

### Test Your Contracts Thoroughly 🧪

Rift also provides a TVM testing framework that makes it easy to test contracts before deploying them to the production network. The framework provides a simple interface for creating message bodies and inspecting the contract's state to ensure correct behavior.

```python
from contracts.jetton_wallet import JettonWallet
from contracts.types import TransferBody


def test_transfer():
    data = create_data().as_cell()
    wallet = JettonWallet.instantiate(data)
    body = create_transfer_body(dest=3)
    res = wallet.send_tokens(
        body.as_cell().parse(), MsgAddress.std(0, 2), int(10e9), 0,
    )
    res.expect_ok()


def test_transfer_no_value():
    data = create_data().as_cell()
    wallet = JettonWallet.instantiate(data)
    body = create_transfer_body(dest=3)
    res = wallet.send_tokens(
        body.as_cell().parse(), MsgAddress.std(0, 2), 0, 0,
    )
    res.expect_error()
```

## Join the TON Revolution 💎
TON has the potential to revolutionize the blockchain world, and Rift is the perfect tool to help developers take advantage of its many benefits. Whether you're just starting out or already have experience with TON, Rift is the perfect way to streamline your development process and bring your ideas to life on TON. So why wait? Start building with Rift today and join the TON revolution! 🚀

Get started now by exploring [Rift's reposittory](https://github.com/sky-ring/rift) and joining the [Skyring Channel](https://t.me/skyring_org) for the latest updates. We're excited to announce that step-by-step guides will be available soon. Stay tuned! ⏳
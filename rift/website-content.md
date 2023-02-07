## Develop Smart Contracts directly in Python!💫

Rift brings easy development of TON Smart Contracts to Python, you can simply take advantage of Python’s syntax and capabilities to develop on TON.

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

## Interact easily with TON!💎
Rift is the ultimate wrapper around TON development tools. You can:
Build messages, sign them (Fift Backend)
Run contract codes, pass objects back and forth (TVM Backend)
Deploy contracts, interact with the network (Tonlibjson Backend)

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

## Battle-test your contracts with Rift!🧪
Rift provides a TVM testing framework, making it easy to test contracts carefully before going into the production. It provides simple interface to create message bodies and inspect contract's state to ensure the correct behavior.

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

from ens import ENS

RPC_URL = "https://eth.llamarpc.com"

w3 = Web3(Web3.HTTPProvider(RPC_URL))
ns = ENS(w3)

def resolve_ens(domain):
    address = ns.address(domain)

    if address is None:
        return None

    return address


def reverse_resolve(address):
    name = ns.name(address)

    if name is None:
        return None

    return name


def main():
    if not w3.is_connected():
        raise ConnectionError(
            "Failed to connect to Ethereum RPC"
        )

    domain = "vitalik.eth"

    print("ENS Domain Resolver")
    print("-" * 35)

    address = resolve_ens(domain)

    if address:
        print(f"Domain:  {domain}")
        print(f"Address: {address}")
    else:
        print("No address found for this domain.")

    print("\nReverse ENS Lookup")

    wallet = address or "0x0000000000000000000000000000000000000000"

    name = reverse_resolve(wallet)

    if name:
        print(f"Address: {wallet}")
        print(f"ENS:     {name}")
    else:
        print("No ENS name found.")


if __name__ == "__main__":
    main()

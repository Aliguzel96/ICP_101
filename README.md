<h2>ICP 101 Notes</h5>
<br>

import Nat "mo:base/Nat";
import Nat64 "mo:base/Nat64";
import Cycles "mo:base/ExperimentalCycles"; //Cycle: ödeme sistemi --son kullanıcıya kadar olan süreç (gas-fee)

//canister: smart contract (canister=actor)

actor HelloCycles {
  //variables

  //let -> immutable(değişmez) javadaki final gibi ??
  //var -> mutable(değişebilir)

  let limit = 10_000_000;

  //functions (query, update)

  public func wallet_balance() : async Nat {
    //return --> return ...;
    //return --> ...
    return Cycles.balance();
    //return Cycles.balance();
  };

  public func wallet_receive() : async { accepted : Nat64 } {
    let available = Cycles.available();
    let accepted = Cycles.accept(Nat.min(available, limit));
    { accepted = Nat64.fromNat(accepted) };

  };

  public func transfer(
    receiver : shared () -> async (),
    amount : Nat) : async { refunded : Nat } {
    Cycles.add(amount);
    await receiver();
    { refunded = Cycles.refunded() };
  };

};

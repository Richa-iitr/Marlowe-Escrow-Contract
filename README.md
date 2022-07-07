## Marlowe Contract

Run/ simulate on [Marlowe Playground](https://marlowe-playground-staging.plutus.aws.iohkdev.io/#/)

<pre>
When
    [Case
        (Deposit
            (Role "Seller")
            (Role "Buyer")
            (Token "" "")
            (ConstantParam "Price")
        )
        (When
            [Case
                (Choice
                    (ChoiceId
                        "Everything is alright"
                        (Role "Buyer")
                    )
                    [Bound 0 0]
                )
                Close , Case
                (Choice
                    (ChoiceId
                        "Report problem"
                        (Role "Buyer")
                    )
                    [Bound 1 1]
                )
                (Pay
                    (Role "Seller")
                    (Account (Role "Buyer"))
                    (Token "" "")
                    (ConstantParam "Price")
                    (When
                        [Case
                            (Deposit
                                (Role "Seller")
                                (Role "Seller")
                                (Token "" "")
                                (ConstantParam "Fees")
                            )
                            (When
                                [Case
                                    (Choice
                                        (ChoiceId
                                            "Dismiss claim"
                                            (Role "Mediator")
                                        )
                                        [Bound 0 0]
                                    )
                                    (Pay
                                        (Role "Buyer")
                                        (Party (Role "Seller"))
                                        (Token "" "")
                                        (ConstantParam "Price")
                                        (Pay
                                            (Role "Seller")
                                            (Party (Role "Mediator"))
                                            (Token "" "")
                                            (ConstantParam "Fees")
                                            Close 
                                        )
                                    ), Case
                                    (Choice
                                        (ChoiceId
                                            "Confirm problem"
                                            (Role "Mediator")
                                        )
                                        [Bound 1 1]
                                    )
                                    (Pay
                                        (Role "Seller")
                                        (Party (Role "Mediator"))
                                        (Token "" "")
                                        (ConstantParam "Fees")
                                        Close 
                                    )]
                                (TimeParam "Mediation deadline")
                                Close 
                            )]
                        (TimeParam "Complaint response deadline")
                        Close 
                    )
                )]
            (TimeParam "Complaint deadline")
            Close 
        )]
    (TimeParam "Payment deadline")
    Close 
</pre>

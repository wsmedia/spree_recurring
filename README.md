SpreeRecurring
==============

A spree extension for implementing recurring billing functionality. This is a
from scratch re-write based on the existing spree\_subscription systems for
pre-1.0, 1.0.0.rc, and pre-deface. The name is changed to avoid
conflits with the existing spree recurring billing gem and the spree
item-based subscription gem.

Architecture
------------

### Backend ###

On the completion of an Order, if the variant.subscribable? is true, a
subscription is created based on the line-item price of the variant. This includes
any price modifications due to variants amounts.

A subscription is attached to:
    * The user who owns the subscription.
    * The original order that was used to create the subscription.
    * The variant that the subscription is based on.
    * A list of additional orders that correspond to the weekly, monthly, daily recurring bills that are paid.
    * A credit card token stored by a given gateway.
    * A list of expiry notices that are sent to the user when their credit card is about to expire.

A subscription also contains the next and previous payment dates, as well as a state machine for
'active', 'past\_due', 'cancelled' states, allowing post-active and post-cancelled states.

NOTE: Subscriptions are considered /prepaid/. The first checkout is considered the first bill. Eventually
it would be nice to have postpaid recurring billing and a 'setup fee' attached to the product.

Products contain a 'subscribable' option and a billing interval option of 'every week, every day, every month'.

### Frontend ####

#### Admin ####

The administrative area contains a subscription list, where an admin can invoke a 'perform recurring billing process'
on a given subscription, as well as search and sort through subscriptions.

The product has a 'subscribable' checkbox on the master variant and all sub variants on creation and editing.

#### Shop ####

The account section contains a Users Subscription list, where they can open a subscription and add/remove/modify
the details of the subscription, including the billing details to use.

The products marked as 'subscribable' have a price listed as X/month,day,year, depending on the product configuration.

Installing
----------

add "gem 'spree\_recurring', :git => ..." to your gemfile.

bundle install

rails g spree\_recurring:install

Copyright (c) 2012 [Sheena Artrip], released under the New BSD License

Order Management Schema Details
This document captures the scenario of simple order management functionality of an online retail store.
Typical purchase scenario: A customer places an order for N products specifying quantity for each line item of the order. 
Every product belongs to a product class (or category). All products ordered
in one order, are shipped to customer’s address (in India or outside) by a shipper in one shipment.
Order can be paid using either Cash, Credit Card or Net Banking.
There can be customers who may not have placed any order. Few customers would have cancelled
their orders (As a whole order, no cancellation of individual item allowed). Few orders may be ‘In
process’ status. There can also be products that were never purchased.
Shippers use optimum sized cartons (boxes) to ship an order, based on the total volume of all
products and their quantities. Dimensions of each product (L, W, H) is also stored in the database. To
keep it simple, all products of an order are put in one single appropriately sized carton for shipping.


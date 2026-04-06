title Instagram Thrift Creator Store
customers [icon:user, color: Blue]{
customer_id serial pk
first_name text not null
last_name text
email text unique not null
password text not null
number text unique not null
avatar_url text
created_at timestamp
updated_at timestamp
}
address[icon: map-pin]{
address_id serial pk
customer_id int fk not null
country_code varchar(2) not null
address_line1 varchar(256) not null
address_line2 varchar(256)
city varchar(50) not null
state varchar(50) not null
pincode varchar(6) not null
country varchar(50) not null
created_at timestamp
updated_at timestamp
}

items[icon:shirt, color: Purple]{
item_id serial pk
name text not null
type text not null ("thrifted","handmade")
description text not null
condition text not null ("new","gently_used","well_used","roughly_used")
created_at timestamp
updated_at timestamp
}
materials[icon:feather, color: Purple]{
material_id serial pk
item_id int fk
type text not null
percentage float not null
created_at timestamp
updated_at timestamp
}

item_variants[icon:layers,iconcolor: Purple]{
item_variant_id serial pk
item_id int fk
item_size text
item_color text
quantity int not null
price int not null
created_at timestamp
updated_at timestamp
}

orders [icon:shopping-cart, color: Orange]{
order_id serial pk
customer_id int fk
address_id int fk
payment_id int fk null
shipping_id int fk null
created_at timestamp
updated_at timestamp
}
order_items[icon:shopping-bag, color: Orange]{
order_item_id serial pk
order_id int fk
item_variant_id int fk
quantity int not null
created_at timestamp
updated_at timestamp
}

payments[icon:credit-card, color: Green]{
payment_id serial pk
payment_method text not null (COD,UPI,credit_card,debit_card,EMI)
provider text not null
provider_payment_id text unique not null
status text not null (pending,paid,partial)
created_at timestamp
updated_at timestamp
}

shipments [icon:truck, color: Blue]{
shipping_id serial pk
weight float not null
price float not null
status text not null (placed, packed, shipped, out_for_delivery, delivered, cancelled, returned)
estimated_date date not null
delivery_date date null
courier_name text not null
tracking_id text unique not null
created_at timestamp
updated_at timestamp
}

//relations
customers.customer_id < address.customer_id
customers.customer_id < orders.customer_id
items.item_id < materials.item_id
items.item_id < item_variants.item_id
orders.order_id < order_items.order_id
orders.address_id - address.address_id
orders.payment_id - payments.payment_id
orders.shipping_id - shipments.shipping_id
order_items.item_variant_id - item_variants.item_variant_id

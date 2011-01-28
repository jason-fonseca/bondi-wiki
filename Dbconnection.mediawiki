ExecuteScalar returns an atomic value
(3:49:30 PM) Ezi-Merch (luke): Execute returns a result set (an array like object)
(3:49:43 PM) Ezi-Merch (luke): ExecuteStream returns an object that behaves like a stream
(3:50:03 PM) Ezi-Merch (luke): so you can do Response.BinaryWrite(Db.ExecuteStream(...
(3:50:12 PM) Ezi-Merch (luke): to serve images from a db
(3:50:23 PM) Ezi-Merch (luke): and ExecuteCallback is the last
(3:50:26 PM) Ezi-Merch (luke): it returns nothing
(3:50:33 PM) Ezi-Merch (luke): 0but you pass it a lambda (anon fn
(3:50:43 PM) Ezi-Merch (luke): and it executes the function for every row
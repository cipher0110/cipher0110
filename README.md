- 👋 Hi, I’m @cipher0110
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
cipher0110/cipher0110 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
<!doctype html>
<html lang="en">

<head>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
        integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js"
        integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo"
        crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js"
        integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1"
        crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"
        integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM"
        crossorigin="anonymous"></script>
</head>

<body>
    <div class="container">
        <div id="show">
        </div>
    </div>
    <script>
        const products = [
            { product: 'Pepsi', price: 25, discount: '0', supplier: 'Bob Inc.', delivery: true },
            { product: 'Dairy milk', price: 40, discount: '5', supplier: 'ABC Supplier', delivery: false },
            { product: 'Maggi', price: 15, discount: '2', supplier: 'James & Co.', delivery: true },
            { product: 'Colgate', price: 80, discount: '5', supplier: 'ABC Limited', delivery: true },
            { product: 'Water', price: 20, discount: '2', supplier: 'James & Co.', delivery: false },
            { product: 'Sandwich', price: 15, discount: '5', supplier: 'ABC Limited', delivery: true },
            { product: 'Tea', price: 10, discount: '0', supplier: 'James & Co.', delivery: false }
        ];
        const purchase = [
            { purchaseId: 'PUR001', product: 'Pepsi', qty: 4, price: 25, supplier: 'Bob Inc.' },
            { purchaseId: 'PUR002', product: 'Colgate', qty: 3, price: 80, supplier: 'James & Co.' },
            { purchaseId: 'PUR003', product: 'Snickers', qty: 2, price: 50, supplier: 'James & Co.' },
            { purchaseId: 'PUR005', product: 'Nescafe', qty: 2, price: 10, supplier: 'ABC Limited' },
            { purchaseId: 'PUR008', product: 'Dairy Milk', qty: 5, price: 40, supplier: 'ABC Limited' },
            { purchaseId: 'PUR009', product: 'Maggi', qty: 5, price: 15, supplier: 'ABC Limited' },
            { purchaseId: 'PUR011', product: 'Snickers', qty: 3, price: 50, supplier: 'James & Co.' },
            { purchaseId: 'PUR016', product: 'Nescafe', qty: 4, price: 10, supplier: 'ABC Limited' },
            { purchaseId: 'PUR017', product: 'Dairy Milk', qty: 4, price: 40, supplier: 'ABC Limited' },
            { purchaseId: 'PUR019', product: 'Pepsi', qty: 5, price: 25, supplier: 'Bob Inc.' },
            { purchaseId: 'PUR021', product: 'Colgate', qty: 4, price: 80, supplier: 'James & Co.' },
            { purchaseId: 'PUR023', product: 'Maggi', qty: 2, price: 15, supplier: 'ABC Limited' }
        ];

        const discountOptions = [
            { display: '2%', value: '2' },
            { display: '5%', value: '5' },
            { display: 'None', value: '0' }
        ];
        const products1 = ['Pepsi', 'Colgate', 'Snickers', 'Nescafe', 'Dairy Milk', 'Maggi']
        const suppliers = ['TABC Limited', 'James & Co.', 'Bob Inc.'];
        let editIndex = -1;
        let myProduct = {};
        let myPurchase = {};
        let error = {};
        show();
        function show(active = 0) {
            let str = makeNavbar();
            active === 1 ? str += showProducts() : str += '';
            active === 2 ? str += showForm() : str += '';
            active === 3 ? str += showForm1() : str += '';
            active === 4 ? str += showPurchase() : str += '';
            console.log(str);
            document.getElementById('show').innerHTML = str;
        }
        function showProduct() {
            editIndex = -1;
            myProduct = {};
            show(1);
        }
        function addProduct() {
            show(2);
        }
        function addPurchase() {
            show(3);
        }
        function showStock() {
            editIndex = -1;
            myPurchase = {};
            show(4);
        }
        function submitPurchase() {
            let purchaseId = document.getElementById('purchaseId').value;
            let quantity = document.getElementById('quantity').value;
            let ps = { purchaseId: purchaseId, quantity: quantity };
            purchase.push(ps);
            showPurchase();
        }

        showPurchase();
        function submitProduct() {
            let prodName = document.getElementById('prodName').value;
            let price = document.getElementById('price').value;
            let supplier = document.getElementById('supplier').value;
            let delivery = document.getElementById('delivery').checked;
            let radios = document.getElementsByClassName('discount');
            let discount = '';
            for (let i = 0; i < radios.length; i++) {
                if (radios[i].checked)
                    discount = radios[i].value;
            }
            let pr = { product: prodName, price: price, discount: discount, supplier: supplier, delivery: delivery };
            if (validate(pr)) {
                editIndex >= 0 ? products[editIndex] = pr : products.push(pr);
                showProducts();
            }
            else {
                myProduct = pr;
                addProduct();
            }

        }
        function validate(pr) {
            error.product = pr.product ? '' : 'Product name is mandatory';
            error.price = (+pr.price) ? '' : 'Enter the price';
            error.discount = pr.discount ? '' : 'choose discount';
            error.supplier = pr.supplier ? '' : 'Select the suplier';
            error.purchaseId = (+pr.purchaseId) ? '' : 'Enter the PurchaseId';
            error.quantity = (+pr.quantity) ? '' : 'Enter the Quantity';
            console.log(error);
            return !(error.product || error.price || error.discount || error.supplier);
        }
        function sort(colNo) {
            switch (colNo) {
                case 0: products.sort((p1, p2) => p1.product.localeCompare(p2.product));
                    break;
                case 1: products.sort((p1, p2) => (+p1.price) - (+p2.price));
                    break;
                case 2: products.sort((p1, p2) => (+p1.discount) - (+p2.discount));
                    break;
                case 3: products.sort((p1, p2) => p1.supplier.localeCompare(p2.supplier));
                    break;
                case 4: products.sort((p1, p2) => p1 ? 1 : -1);
                    break;
            }
            showProduct();
        }
        function deleteProduct(index) {
            products.splice(index, 1);
            showProduct();
        }
        function editProduct(index) {
            editIndex = index;
            myProduct = products[index];
            addProduct();
        }
        function deletePurchase(index) {
            purchase.splice(index, 1);
            showStock();
        }
        function editPurchase(index) {
            editIndex = index;
            myPurchase = purchase[index];
            addPurchase();
        }
        function makeNavbar(active = 0) {

            let str = '<nav class="navbar navbar-expand-lg navbar-light bg-light">';
            str += '<a class="navbar-brand" href="#">Store System</a>';
            str += '<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavDropdown">';
            str += '<span class="navbar-toggler-icon"></span>';
            str += '</button>';
            str += '<div class="collapse navbar-collapse" id="navbarNavDropdown">';
            str += ' <ul class="navbar-nav">';
            let active1 = active === 1 ? 'active' : '';
            let active2 = active === 2 ? 'active' : '';
            let active3 = active === 3 ? 'active' : '';
            let active4 = active === 4 ? 'active' : '';
            str += '<li class="nav-item ' + active1 + '">';
            str += '<a class="nav-link" onclick="showProduct()">Show Product <span class="sr-only">(current)</span></a>';
            str += ' </li>';
            str += '<li class="nav-item ' + active2 + '">';
            str += '<a class="nav-link" onclick="addProduct()">Add Product</a>';
            str += '</li>';
            str += '<li class="nav-item ' + active3 + '">';
            str += '<a class="nav-link" onclick="addPurchase()">Purchase</a>';
            str += '</li>';
            str += '<li class="nav-item ' + active4 + '">';
            str += '<a class="nav-link" onclick="showStock()">Stock</a>';
            str += '</li>';
            str + '</ul>';
            str += '</div>';
            str += '</nav>';
            return str;
        }
        function showProducts() {
            const arr = products.map((pr, index) => {
                let { product, price, discount, supplier, delivery } = pr;
                let str = '<div class="row border">';
                str += '<div class="col-3">' + product + '</div>';
                str += '<div class="col-1">' + price + '</div>';
                str += '<div class="col-2">' + discount + '</div>';
                str += '<div class="col-2">' + supplier + '</div>';
                str += '<div class="col-2">' + delivery + '</div>';
                str += '<div class="col-2"><button class="btn btn-secondary btn-sm" onclick="editProduct(' + index + ')">Edit</button><button class="btn btn-danger btn-sm" onclick="deleteProduct(' + index + ')">Delete</button></div>';
                str += '</div>';
                return str;
            });
            let table = '<h1 class="text text-center">List of Products</h1>';
            table += '<div class="row bg-dark text-white">';
            table += '<div class="col-3" onclick="sort(0)">Product</div>'
            table += '<div class="col-1" onclick="sort(1)">Price</div>';
            table += '<div class="col-2" onclick="sort(2)">Discount</div>';
            table += '<div class="col-2" onclick="sort(3)">Supplier</div>';
            table += '<div class="col-2" onclick="sort(4)">Delivery</div>';
            table += '<div class="col-2"></div>';
            table += '</div>';
            table += arr.join('');
            return table;

        }
        function showPurchase() {
            const arr = purchase.map((pr, index) => {
                let { purchaseId, product, qty, price, supplier } = pr;
                let str = '<div class="row border">';
                str += '<div class="col-2">' + purchaseId + '</div>';
                str += '<div class="col-2">' + product + '</div>';
                str += '<div class="col-2">' + qty + '</div>';
                str += '<div class="col-2">' + price + '</div>';
                str += '<div class="col-2">' + supplier + '</div>';
                str += '<div class="col-2"><button class="btn btn-secondary btn-sm" onclick="editPurchase(' + index + ')">Edit</button><button class="btn btn-danger btn-sm" onclick="deletePurchase(' + index + ')">Delete</button></div>';
                str += '</div>';
                return str;
            });
            let table = '<h1 class="text text-center">List of Purchase</h1>';
            table += '<div class="row bg-dark text-white">';
            table += '<div class="col-2" >PurchaseID</div>'
            table += '<div class="col-2" >Product</div>'
            table += '<div class="col-2" >Quantity</div>';
            table += '<div class="col-2" >Price</div>';
            table += '<div class="col-2" >Supplier</div>';
            table += '<div class="col-2"></div>';
            table += '</div>';
            table += arr.join('');
            return table;

        }
        function showForm() {
            let { product = '', price = '', quantity = '', discount = '', supplier = '', delivery = false } = myProduct;
            let str = '<h1 class="text text-center">Add New Product</h1>';
            str += makeTextField('ProdName', 'Product Name', 'Enter The Product Name', product, error.product);
            str += makeTextField('price', 'Price', 'Enter the price', price, error.price);
            str += makeRadios(discountOptions, 'Discount', 'discount', discount, error.discount);
            str += makeDropdown('supplier', suppliers, 'Select Suppliers', supplier, error.supplier);
            str += makeCheckbox('delivery', 'delivery available', delivery);
            str += '<button class="btn btn-primary" onclick="submitProduct()">Submit</button>';
            return (str);
        }
        function makeTextField(id, label, placeholder = '', value = '', err = '') {
            let str = '<div class="form-group">';
            str += '<label>' + label + '</label>';
            str += '<input type="text" class="form-control"id="' + id + '"placeholder="' + placeholder + '"value="' + value + '">';
            str += err ? '<span class="text-danger">' + err + '</span>' : '';
            str += '</div>';
            return str;
        }
        function showForm1() {
            let { purchaseId = '', product = '', quantity = '', value = '', err = '' } = myPurchase;
            let str = '<h1 class="text text-center">New Purchase Form</h1>';
            str += makeTextField1('ProdId', 'Purchase Id', 'Enter The Purchase Id', purchaseId, err.purchaseId);
            str += makeDropdown('products1', products1, 'Select product', products1, err.products1);
            str += makeTextField1('quantity', 'Quantity', 'Enter the Quantity', quantity, err.quantity);
            str += '<button class="btn btn-primary" onclick="submitPurchase()">Submit</button>';
            return (str);
        }
        function makeTextField1(id, label, placeholder = '', value = '', err = '') {
            let str = '<div class="form-group">';
            str += '<label>' + label + '</label>';
            str += '<input type="text" class="form-control"id="' + id + '"placeholder="' + placeholder + '"value="' + value + '">';
            str += err ? '<span class="text-danger">' + err + '</span>' : '';
            str += '</div>';
            return str;
        }
        function makeRadios(arr, label, name, radioValue = '', err = '') {
            const arr1 = arr.map(opt => {
                let { display, value } = opt;
                let checked = radioValue === value ? 'checked' : '';
                let str = '<div class="form-check form-check-inline">';
                str += '<input class="form-check-input"type="radio"name="' + name + '"value="' + value + '"' + checked + '>';
                str += '<label class="form-check-label">' + display + '</label>';
                str += '</div>';
                return str;
            });
            let s1 = '<div class="form-check form-check-inline">';
            s1 += '<label class="form-check-label">' + label + '</label>';
            s1 += err ? '<br/><span class="text-danger">' + err + '</span>' : '';
            s1 += '</div>';
            s1 += arr1.join('');
            return s1;
        }
        function makeDropdown(id, options, header, selvalue = '', err = '') {
            const arr1 = options.map(opt => {
                let selected = opt === selvalue ? 'selected' : '';
                return '<option' + selected + '>' + opt + '</option>';
            });
            let s1 = '<div class="form-group">';
            s1 += '<select id="' + id + '"class="form-control">';
            let selectedHeader = selvalue ? '' : 'selected';
            s1 += '<option value=""disabled selected>' + selectedHeader + '</option>';
            s1 += err ? '<span class="text-danger">' + err + '</span>' : '';
            s1 += arr1.join('');
            s1 += '</select>';
            s1 += '</div>';
            return s1;
        }
        function makeCheckbox(id, label, checked) {
            let c1 = checked ? 'checked' : '';
            let str = '<div class="form-check">';
            str += '<input type="checkbox"class="form-check-input"id="' + id + '"' + c1 + '>';
            str += '<label class="form-check-label">' + label + '</label>';
            str += '</div>';
            return str;
        }
    </script>
</body>

</html>

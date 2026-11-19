# WooCommerce and Odoo 18 installation guide

Extract the outer download once. It contains two software packages and two language guides.

## Install on WordPress

1. Sign in to the staging WordPress dashboard. Activate WooCommerce first. The connector requires WordPress 6.5+, WooCommerce 8.2+ and PHP 8.0+.
2. Open **Plugins → Add New Plugin → Upload Plugin**. Select **WordPress-Plugin-2.1.6.zip** and keep it zipped.
3. Click **Install Now**, then **Activate Plugin**. Open **WooCommerce → Odoo Sync**.

## Install on Odoo 18

Use a fresh staging database. The previous connector and this module share database model names and cannot be installed together.

1. Extract **Odoo-18-Module-18.0.1.0.0.zip**. It contains one folder named `woo_commerce_connector`. Keep the folder name and its contents together.
2. **Odoo.sh** — Commit that folder at the root of the GitHub branch connected to staging. Confirm the repository shows `woo_commerce_connector/__manifest__.py`. Wait for the build to succeed and open the staging database.
   
   **Self-hosted Odoo** — Copy the same folder into a directory listed in `addons_path`. Ensure the Odoo Python environment has Requests and Pillow, then restart Odoo.
3. In Odoo, enable developer mode. Under **Apps**, choose **Update Apps List** and confirm. Search for **WooCommerce Connector (Odoo 18 Module)** and click **Activate**.
4. Grant the setup administrator the **Woo Connector Manager** group. Open **WooCommerce → Connections**.

## Connect and test

1. In WooCommerce → Odoo Sync, copy the store identifier and set the Odoo event URL to https://odoo.example.com/mw-sync/v2/event. In Odoo → WooCommerce → Connections, use the same identifier and set the stock URL to https://shop.example.com/wp-json/mw-sync/v2/stock. Replace the example hosts with yours. Match the outgoing secret on each side to the incoming secret on the other. Use two different random secrets of at least 32 characters. Keep them out of Git.
2. Assign a dedicated internal Odoo user the Woo Connector Service group and access to the intended company. Configure the warehouse, pricelist, tax mappings, payment journals and shipping/fee service product. Configure the order cutover before enabling live sync. Leave automatic fulfillment blank unless a particular Woo status should validate ready deliveries and trigger invoicing.
3. Enable event sync and test a product with a unique SKU, a variation and a new order. Check quantities, pricing, tax and the reservation acknowledgement in both systems. Enable stock publishing after the mapping and warehouse quantities agree. Test cancellations and refunds on staging before enabling production order processing.

Odoo: Adding SignUp and CheckOut Button
---------------------------------------

website_sale_suggest_create_account/__manifest__.py
"data": ["views/website_sale.xml", "views/assets.xml"],

分成 suggest_create_account 和 suggest_login 兩個變數

website_sale_suggest_create_account/views/website_sale.xml

        <xpath expr="//div[@id='wrap']" position="before">
            <t t-set="user_authenticated" t-value="user_id != website.user_id" />
            <t
                t-set="signup_allowed"
                t-value="user_id._get_signup_invitation_scope() == 'b2c'"
            />
            <t
                t-set="can_checkout"
                t-value="website_sale_order and
                                             website_sale_order.website_order_line"
            />
            <t
                t-set="suggest_create_account"
                t-value="not user_authenticated and signup_allowed and can_checkout"
            />
            <t
                t-set="suggest_login"
                t-value="not user_authenticated and not signup_allowed and can_checkout"
            />
        </xpath>

`website_sale/controllers/main.py <https://github.com/odoo/odoo/tree/13.0/addons/website_sale/views>`_

  @http.route(['/shop/cart'], type='http', auth="public", website=True, sitemap=False)
  def cart(self, access_token=None, revive='', **post):
      ....
      return request.render("website_sale.cart", values)


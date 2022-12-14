# custom-wordpress-menu-create
# Put this code in function.php<br/>
function wp_get_menu_array($current_menu) {<br/>
    $array_menu = wp_get_nav_menu_items($current_menu);<br/>
    $menu = array();<br/>
    foreach ($array_menu as $m) {<br/>
        if (empty($m->menu_item_parent)) {<br/>
            $menu[$m->ID] = array();<br/>
            $menu[$m->ID]['ID']          =   $m->ID;<br/>
            $menu[$m->ID]['title']       =   $m->title;<br/>
            $menu[$m->ID]['url']         =   $m->url;<br/>
            $menu[$m->ID]['children']    =   array();<br/>
        }<br/>
    }<br/>
    $submenu = array();<br/>
    foreach ($array_menu as $m) {<br/>
        if ($m->menu_item_parent) {<br/>
            $submenu[$m->ID] = array();<br/>
            $submenu[$m->ID]['ID']       =   $m->ID;<br/>
            $submenu[$m->ID]['title']    =   $m->title;<br/>
            $submenu[$m->ID]['url']      =   $m->url;<br/>
            $menu[$m->menu_item_parent]['children'][$m->ID] = $submenu[$m->ID];<br/>
        }<br/>
    }<br/>
    return $menu;<br/>
}<br/>
# Call this anywhere in your theme like this.<br/>
#<nav>
#      <ul>
        <?php
        $menu_items = wp_get_menu_array('menu_name');
        foreach ($menu_items as $item) : ?>
          <li>
            <a href="<?= $item['url'] ?>" title="<?= $item['title'] ?>"><?= $item['title'] ?></a>
            <?php if( !empty($item['children']) ):?>
            <ul class="sub-menu">
              <?php foreach($item['children'] as $child): ?>
                <li class="b-main-header__sub-menu__nav-item">
                  <a href="<?= $child['url'] ?>" title="<?= $child['title'] ?>"><?= $child['title'] ?></a>
                </li>
              <?php endforeach; ?>
            </ul>
            <?php endif; ?>
          </li>
        <?php endforeach; ?>
     <ul>
</nav>

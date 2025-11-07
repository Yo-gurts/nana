## TODO

- [ ] https://github.com/sophgo/linux_5.10/tree/sg200x-dev/scripts/dtc/include-prefixes 路径下的文件应该是软链接而不是实体文件夹。导致 `dts` 中使用 `#include <dt-bindings/input/input.h>` 时找不到头文件。内部代码是软链接~ github 的v4.2.0 分支也不存在该问题~
- [ ] 同时检查其他文件，是否存在类似的问题~ 软连接变实体文件或文件夹~

## NOTE

- [x] 在没有删除设备树上的节点 `cv_sd` 时，发现 cvi_board_init() 中设置的 SD0_DX 的 pinmux 配置会被重置~，这个是为了保证SD卡一定能用，uboot, kernel 中只要设备树匹配到了 sd 卡节点，就会重新设置 pinmux 配置~
  - 解决方法：删除设备树中的 sd 卡节点。
- [x] `/delete-node/ spi0@04180000;` 删除设备节点时报错，因为 `cv180x_base.dtsi` 中有 `aliases` 节点引用了 `spi0@04180000` 节点，导致删除时报错~
  - 解决方法：同时删除 `aliases` 中的引用~

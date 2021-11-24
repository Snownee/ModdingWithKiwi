# 动态配方

```java
public class TestRecipe extends DynamicShapedRecipe {

	public TestRecipe(ResourceLocation idIn) {
		super(idIn);
	}

	// optional
	@Override
	public boolean matches(CraftingContainer inv, Level worldIn) {
		int[] pos = search(inv);
		if (pos == null)
			return false;
		ItemStack stack = item('#', inv, pos);
		return stack.hasTag() && stack.getTag().contains("Rarity");
	}

	@Override
	public ItemStack assemble(CraftingContainer inv) {
		ItemStack res = recipeOutput.copy();
		int[] pos = search(inv);
		ItemStack stack = item('#', inv, pos);
		if ("SSR".equals(stack.getTag().getString("Rarity")))
			res.grow(res.getCount());
		return res;
	}

	@Override
	public RecipeSerializer<?> getSerializer() {
		return null; //TODO your serializer
	}

	public static class Serializer extends DynamicShapedRecipe.Serializer<TestRecipe> {

		@Override
		public TestRecipe fromJson(ResourceLocation pRecipeId, JsonObject pSerializedRecipe) {
			TestRecipe recipe = new TestRecipe(pRecipeId);
			fromJson(recipe, pSerializedRecipe);
			//TODO customize recipe
			return recipe;
		}

		@Override
		public TestRecipe fromNetwork(ResourceLocation pRecipeId, FriendlyByteBuf pBuffer) {
			TestRecipe recipe = new TestRecipe(pRecipeId);
			fromNetwork(recipe, pBuffer);
			//TODO customize recipe
			return recipe;
		}

	}
}
```

```json
{
  "type": "mymod:test",
  "pattern": [
    "#  ",
    "## ",
    "###"
  ],
  "key": {
    "#": {
      "item": "dirt"
    }
  },
  "result": {
    "item": "diamond"
  }
}
```